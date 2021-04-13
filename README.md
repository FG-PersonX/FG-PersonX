# The FG-PersonX Dataset

![overview](https://github.com/FG-PersonX/FG-PersonX/blob/main/images/overview.PNG)

In this work, we develop a new large-scale synthetic dataset, named **FG-PersonX**, to support the study of fine-grained person re-identification (FG person re-ID) where people wear similar clothes such as uniforms.  FG-PersonX dataset contains 1,000 identities of five groups: students, medical personnel,construction workers, sportsmen, and security personnel. Over 40K images are generated under different indoor and outdoor scenes. We also provide a data synthesizing engine that can generate customized data in different environments by changing visual factors such as illumination, backgrounds, and viewpoints. Besides, We have performed extensive evaluations using FG-PersonX for benchmarking purposes.

To help know this work quickly, there are some summarized contents as follows.

## List of contents

* [1. Data Synthesizing System](#1-Data-Synthesizing System)
* [2. Dataset introduction](#2-Dataset-introduction)
* [3. Experiment](#3-Experiment)

## 1. Data Synthesizing System

![synthesizing_system](https://github.com/FG-PersonX/FG-PersonX/blob/main/images/synthesizing_system.PNG)
By using the Unity engine, we create a controllable system to generate images with 3D models. The assets include 3D models of person and scene. We also provide interfaces with editable parameters that can be used to modify commonly studied visual factors in person re-ID, such as viewpoint, illumination, and background.<br>
**Identities.** There are 1,000 hand-crafted identities of 5 professions. All of them are designed to wear uniforms, and only subtle differences can be used to distinguish them. To ensure diversity, we hand-craft the person models with different genders, skin colors, ages, hairstyles. The picture above shows sample images of different identities.<br>
**Scenes.** Five scenes are provided corresponding to different professions, which includes school, hospital, factory/construction site, playground and city block. Meanwhile, we provide an interface that allows users to add other scenes into the system based on their demands. <br>
**Visual Factors.** The selections of illumination contain directional light (sunlight), point light, spotlight, and area light. Backgrounds will change with the field of view (FoV) of the camera and position of the person in the 3D scene, and more variance in the 3D scene means images can be generated with more diverse backgrounds.<br>

## 2.  Dataset introduction 

Based on the above system, the FG-PersonX image dataset is generated as a benchmark, and the settings of the datasets are introduced below.<br>
**Cameras.** We arrange 2 cameras at different positions in each scene for a total of 10 cameras. Users can modify the number of cameras arbitrarily for their own tasks. The resolution of cameras is set to 1024x768.<br>
**Viewpoints.** For each identity, we change its rotation angle (relative to the camera) in 10° to capture 35 images under each camera, and then randomly sample 5 or 6 images to formulate our dataset. Finally, there are 49,291 images are generated in the current FG-PersonX dataset.<br>

## 3. Experiment

![experiments](https://github.com/FG-PersonX/FG-PersonX/blob/main/images/experiments.PNG)
We  evaluate  commonly  used  techniques  of  person  re-ID using the FG-PersonX dataset and report the results forbenchmarking purposes.<br>
**Models.** We select several commonly used person re-id models from three high-started Github repositories, [Person-reID-baseline](https://github.com/layumi/Person_reID_baseline_pytorch), [reid-strong-baseline](https://github.com/michuanhaohao/reid-strong-baseline) and [deep-person-reid](https://github.com/KaiyangZhou/deep-person-reid).<br>
**Evaluation Metric.** Cumulative matching characteristics (CMC) and mean average precision (mAP) are used for evaluation.<br>



