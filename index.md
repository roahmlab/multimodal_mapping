---
# Front matter. This is where you specify a lot of page variables.
layout: default
title:  "You've Got to Feel It To Believe It: Multi-Modal Bayesian Inference for Semantic and Property Prediction"
date:   2024-02-08 13:24:00 -0400
description: >- # Supports markdown
  Using multiple sensing modalities improves semantic predictions and enables complex task completion.
show-description: true

# Add page-specifi mathjax functionality. Manage global setting in _config.yml
mathjax: true
# Automatically add permalinks to all headings
# https://github.com/allejo/jekyll-anchor-headings
autoanchor: true

# Preview image for social media cards
image:
  path: https://raw.githubusercontent.com/ParkerEwen5441/github.io-multimodal_mapping/main/web_elements/pitch.png
  height: 100
  width: 256
  alt: Random Landscape

# Only the first author is supported by twitter metadata
authors:
  - name: Parker Ewen
    url: https://parkerewen.com
    email: pewen@umich.edu
  - name: Hao Chen
    email: haochern@umich.edu
  - name: Yuzhen Chen
    email: yuzhench@umich.edu
  - name: Anran Li
    email: anranli@umich.edu
  - name: Anup Bagali
    email: abagali@umich.edu
  - name: Gitesh Gunjal
    email: gitesh@umich.edu
  - name: Ram Vasudevan
    email: ramv@umich.edu

# If you just want a general footnote, you can do that too.
# See the sel_map and armour-dev examples.
author-footnotes:
  All authors affiliated with the Robotics Institute and department of Mechanical Engineering of the University of Michigan, Ann Arbor.

links:
  - icon: arxiv
    icon-library: simpleicons
    text: arXiv
    url: https://arxiv.org/abs/2402.05872
  - icon: github
    icon-library: simpleicons
    text: Code
    url: https://github.com/ParkerEwen5441/KinfuROS

# End Front Matter
---

{% include sections/authors %}
{% include sections/links %}


<!-- # [Content](#content)
<div markdown="1" class="content-block grey justify no-pre">
some text

Try clicking this heading, this shows the manually defined header anchor, but if you do this, you should do it for all headings.
</div>

I made this look right by adding the `no-pre` class.
If you don't include `markdown="1"` it will fail to render any markdown inside.

You can also make fullwidth embeds (this doesn't actually link to any video)
 -->
# Demo Video
<div class="fullwidth">
<video controls="" width="100%">
    <source src="https://github.com/ParkerEwen5441/github.io-multimodal_mapping/assets/48282126/bb4d10af-1db5-4dc7-bfc1-bc8a6cdf3bda">
</video>
</div>

<div markdown="1" class="content-block grey justify">
# Abstract

Robots must be able to understand their surroundings to perform complex tasks in challenging environments and many of these complex tasks require estimates of physical properties such as friction or weight.
Estimating such properties using learning is challenging due to the large amounts of labelled data required for training and the difficulty of updating these learned models online at run time.
To overcome these challenges, this paper introduces a novel, multi-modal approach for representing semantic predictions and physical property estimates jointly in a probabilistic manner.
By using conjugate pairs, the proposed method enables closed-form Bayesian updates given visual and tactile measurements without requiring additional training data.
The efficacy of the proposed algorithm is demonstrated through several hardware experiments. 
In particular, this paper illustrates that by conditioning semantic classifications on physical properties, the proposed method quantitatively  outperforms state-of-the-art semantic classification methods that rely on vision alone.
To further illustrate its utility, the proposed method is used in several applications including to represent affordance-based properties probabilistically and a challenging terrain traversal task using a legged robot.
In the latter task, the proposed method represents the coefficient of friction of the terrain probabilistically, which enables the use of an on-line risk-aware planner that switches the legged robot from a dynamic gait to a static, stable gait when the expected value of the coefficient of friction falls below a given threshold.
Videos of these case studies are shown above.

<p align="center">
<img src="https://raw.githubusercontent.com/ParkerEwen5441/github.io-multimodal_mapping/main/web_elements/pitch.png" class="img-responsive" alt="" width="500" height="500">
</p>

The method proposed in this paper jointly estimates semantic classifications and physical properties by combining visual and tactile data into a single semantic mapping framework. 
RGB-D images are used to build a metric-semantic map that iteratively estimates semantic labels. 
A property measurement is taken which in turn updates both the semantic class predictions and physical property estimates. 
In the depicted example, the robot is unsure if the terrain in front of it is snow or ice from vision measurements alone (prior estimates) which dramatically affects the coefficient of friction and the associated gait that can be applied to safely traverse the terrain.
The robot uses a tactile sensor attached to its manipulator to update its coefficient of friction estimation (posterior estimates), which then enables it to change gaits to cross the ice safely.
</div>

# Method

A flow diagram illustrating our proposed mutli-modal mapping algorithm.
A semantic classification algorithm predicts pixel-wise classes from RGB images that are then projected into a common mapping frame using the aligned depth image, camera intrinsics, and estimated camera pose. 
This semantic point cloud is used to build a metric-semantic map. 
When a property measurement is taken, the method of moments is used to update the semantic and property estimates jointly.

![Flow diagram for multi-modal mapping](https://raw.githubusercontent.com/ParkerEwen5441/github.io-multimodal_mapping/main/web_elements/RSS_flow_diagram_updated.jpeg "Flow Diagram")

We use a custom implementation of the [SegFormer network](https://github.com/NVlabs/SegFormer) trained on the [Dense Material Segmentation Dataset](https://github.com/apple/ml-dms-dataset).
The output of the network is then post-processed with a segment-based voting scheme using [FastSAM](https://github.com/CASIA-IVA-Lab/FastSAM).
When a property measurement is taken, the method of moments updates a region of the geometric representation segmented using FastSAM.
This incentivizes regions with spatial proximity and visual similarity to be updated using a single measurement rather than requiring one measurement for each voxel, making the algorithm more efficient. 
For hardware demonstrations, static friction measurements are taken using a force-torque sensor which measures contact forces during the motion of an end-effector against a surface.


<div markdown="1" class="content-block grey justify">
# Simulation Results

We validate our approach in simulation and demonstrate property measurements improve semantic predictions.
We use 1800 images and ground-truth semantic labels taken from the testing set of the [Dense Material Segmentation Dataset](https://github.com/apple/ml-dms-dataset) and a pre-trained semantic segmentation neural network is used to predict semantic labels.
Misclassified pixels in each image are randomly selected and a friction measurement is drawn from a Gaussian distribution using the corresponding ground-truth semantic label.

![Simulation results for multi-modal mapping](https://raw.githubusercontent.com/ParkerEwen5441/github.io-multimodal_mapping/main/web_elements/sim_results.png "Simulation Results")

Results for a single simulated experiment are shown above.
The image (a) and ground truth semantic labels (b) are from the Dense Material Segmentation Dataset. The semantic segmentation predictions (c) do not classify parts of the desk as wood.
The method of moments then computes the correct posterior semantic label (d).

The mean pixel-wise accuracy of the semantic segmentation network over this dataset without applying the proposed algorithm is 27.2\%.
The mean pixel accuracy of the posterior semantic predictions output by Algorithm \ref{alg:moments} is 33.0\%, an increase of 21.3\%, demonstrating increased semantic prediction performance compared to semantic predictions using vision alone.
This experiment highlights that by conditioning semantic classifications on physical properties, one can generally improve the accuracy of visual semantic classifications.

</div>

# Hardware Demonstrations

We further validate our method on several hardware demonstrations and compare against existing semantic mapping and property estimation approaches.

The end-effector of a Kinova Gen 3 robotic arm and a Spot Arm are used to collect friction measurements using built-in wrist-mounted force-torque sensors.
For this experiment, 5 frames from the RGB-D stream are used to initialize the semantic TSDF map.
Each RGB-D image is semantically segmented using the trained network and projected into the global coordinate frame using the aligned depth image.
The recursive vision-based semantic update is applied for each semantic point cloud.
This initializes the Dirichlet parameters used to compute the initial semantic classification weights for the measurement likelihood.

![Hardware demonstrations for multi-modal mapping](https://raw.githubusercontent.com/ParkerEwen5441/github.io-multimodal_mapping/main/web_elements/hardware_results.png "Hardware Demonstrations")

We compare our approach to the recursive semantic mapping approach of [selmap](https://github.com/roahmlab/sel_map) which only uses vision.
The same SegFormer-FastSAM semantic segmentation network is used for both methods.
As shown in in the above demonstration, the network incorrectly classifies various objects in the scenes.
When only vision-based semantic classifications are considered, the erroneous predictions from the network are unable to be corrected by selmap.
Using the proposed method, a user specifies the location for friction measurements to be taken.
The method of moments then computes the posterior semantic classification weights using the friction measurements as input.
This corrects the expected semantic classification.
We ran the experiment on two indoor scenes with two friction measurements each and show the semantic prediction accuracy increases using the proposed method and matches the ground-truth semantic labels.

We demonstrate the utility of our proposed property estimation method using a case study involving a challenging legged locomotion traversal task of crossing an icy surface and, likewise, on a case study for affordance-based property estimation.
These two case studies are presented in the above video.

<div markdown="1" class="content-block grey justify">
# Citation

This project was developed in [Robotics and Optimization for Analysis of Human Motion (ROAHM) Lab](http://www.roahmlab.com/) at University of Michigan - Ann Arbor.

```bibtex
@article{ewen2024feelit,
  author  = "Ewen, Parker and Chen, Hao and Chen, Yuzhen and Li, Anran and Bagali, Anup and Gunjal, Gitesh and Vasudevan, Ram",
  title   = "You've Got to Feel It To Believe It: Multi-Modal Bayesian Inference for Semantic and Property Prediction",
  journal = "Robotics: Science and Systems",
  year    = 2024
}
```
</div>
