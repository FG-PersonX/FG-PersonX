# The FG-PersonX Dataset

![overview](https://github.com/FG-PersonX/FG-PersonX/blob/main/images/overview.PNG)

In this work, we develop a new large-scale synthetic dataset, named **FG-PersonX**, to support the study of fine-grained person re-identification (FG person re-ID) where people wear similar clothes such as uniforms.  FG-PersonX dataset contains 1,000 identities of five groups: students, medical personnel,construction workers, sportsmen, and security personnel. Over 40K images are generated under different indoor and outdoor scenes. We also provide a data synthesizing engine that can generate customized data in different environments by changing visual factors such as illumination, backgrounds, and viewpoints. Besides, We have performed extensive evaluations using FG-PersonX for benchmarking purposes.

To help know this work quickly, there are some summarized contents as follows.

## List of contents

* [1. Data Synthesizing System](#1-Data Synthesizing System)
* [2. Dataset introduction](#2-dataset-introduction)
* [3. Experiment](#3-Experiment)

## 1. Data Synthesizing System

![synthesizing_system](https://github.com/FG-PersonX/FG-PersonX/blob/main/images/synthesizing_system.PNG)
By using the Unity engine, we create a controllable system to generate images with 3D models. The assets include 3D models of person and scene. We also provide interfaces with editable parameters that can be used to modify commonly studied visual factors in person re-ID, such as viewpoint, illumination, and background.<br>
**Identities.** There are 1,000 hand-crafted identities of 5 professions. All of them are designed to wear uniforms, and only subtle differences can be used to distinguish them. To ensure diversity, we hand-craft the person models with different genders, skin colors, ages, hairstyles. The picture above shows sample images of different identities.<br>
**Scenes.** Five scenes are provided corresponding to different professions, which includes school, hospital, factory/construction site, playground and city block. Meanwhile, we provide an interface that allows users to add other scenes into the system based on their demands. <br>
**Visual Factors.** The selections of illumination contain directional light (sunlight), point light, spotlight, and area light. Backgrounds will change with the field of view (FoV) of the camera and position of the person in the 3D scene, and more variance in the 3D scene means images can be generated with more diverse backgrounds.<br>

## 2.  Dataset introduction 

Based on the above system, the FG-PersonX image dataset is generated as a benchmark, and the settings of the datasets are introduced below.<br>
**Cameras.** We arrange 2 cameras at different positions in each scene for a total of 10 cameras. Users can modify the number of cameras arbitrarily for their own tasks. The resolution of cameras is set to 1024x768.
**Viewpoints.** For each identity, we change its rotation angle (relative to the camera) in 10° to capture 35 images under each camera, and then randomly sample 5 or 6 images to formulate our dataset. Finally, there are 49,291 images are generated in the current FG-PersonX dataset.

## 3. Experiment

Based on the PersonX engine, this paper makes an early attempt in studying a particular factor, **viewpoint**.

![Fig 4](https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/fig-dfv.jpg)

Here, we denote viewpoint as the pedestrian rotation angle (as shown in above Figure). Since different views of a person contain different details, the viewpoint of a person influences the visual information contained in the image, which is directly related to the performance of the algorithm. Therefore, we investigate the exact influence of viewpoint on the system from **three** aspects. 


## 3.1. How do viewpoint distributions in the training set affect model learning

### 3.1.1 Experiment design

* Control group 1. We randomly select half (18 out of 36) or a quarter (9 out of 36) images of each identity
  for training.
* Control group 2. The training set is constituted by randomly selecting half (18 out of 36) or a quarter (9 out of 36) viewpoints for each identity.
  There is an example of Control group 1 and Control group2 in the following figure.

<div align="center">
  <img src="https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/v3-valse1.jpg" width="600">
</div>


* Experimental group 1. Train with two orientations. The training images exhibit two orientations, left+right or front+back. The training set is thus half of the original training set.
* Experimental group 2. Train with one orientation. The training set has one orientation, i.e., left, right, front, or back. The training set becomes a quarter of the size of the original training set.

### 3.1.2 Experiment results

The Re-ID accuracy (mAP, %) when the training set has missing orientations/viewpoints is shown in following figure.

![Fig 6](https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/Train-1.jpg)

Here, **A** and **B**: we use two orientations for training, *e.g.,*, training with left and right orientations only (see the definition of viewpoint). **C**: we train with one orientation only, *i.e.,* left, right, front, or back orientation. **D**: Impact of missing continuous viewpoints on PersonX46. The horizontal axis is the remaining number of viewpoints and vertical axis is the mAP. In the experimental group, continuous viewpoints are removed. The number on this curve denotes the remaining number of viewpoints.
“n.s.” represents that the difference between results is not statistically significant.  ★ corresponds to statistically significant. ★★★ means the difference between results is statistically very significant.

### 3.1.3 Subsection conclusions

* Missing viewpoints compromises training.
* Missing continuous viewpoints are more detrimental than missing randomly viewpoints.
* When limited training viewpoints are available, left/right orientations allow models to be better trained than front/back orientations.


## 3.2. How do true match viewpoints in the gallery affect retrieval

### 3.2.1 Experiment design

We denote the viewpoint of a query and its true match as \theta_{t} and \theta_{q}, respectively.

* Experimental group 1. The three true matches whose viewpoints are viewpoint of query <img src="https://latex.codecogs.com/gif.latex?\pm" title="\pm" /> 10 are removed (set as “junk”).
* Control group 1. Three true matches are randomly removed from the gallery. The illustrations are shown as follows:  

<div align="center">
  <img src="https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/ed2.jpg" width="600">
</div>


Similarly, experimental group 2 and 3, as well as control group 2 and 3 means remove Five (\theta_{t} \in [\theta_{q}-20, \theta_{q}+20] ) and Nine (\theta_{t} \in [\theta_{q}-40, \theta_{q}+40] ) images, respectively.

### 3.2.2 Experiment results

Experiments are conducted on PersonX45, PersonX46, PersonX46-lr as well as Market-1203. 

<div align="center">
  <img src="https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/gallery.jpg" width="600">
</div>


### 3.2.3 Subsection conclusions

* True matches whose viewpoints are dissimilar to the query are harder to be retrieved than true matches with a similar viewpoint to the query.
* The above problem becomes more severe when the environment is challenging, *e.g.,* complex background and low resolution.


## 3.3. How does the query viewpoint influence the retrieval?

### 3.3.1 Experiment design

We train a model on the original training set comprised of every viewpoint. We modify the query viewpoints to see its effect during testing. Specifically, the viewpoint of a probe image can be set to the **due left (0)**, **due front (90)**, **due right (180)** or **due back (270)** to represent different sides of person. During retrieval, we assume only one true match in gallery; the true
match contains the same person as the query, and its viewpoint is between 0 and 350. Viewpoints of the distractor gallery images are images of all other persons. Taking using due left as query viewpoint as an example, the setting is shown as follows: 

<div align="center">
  <img src="https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/ed3.jpg" width="700">
</div>


### 3.3.2 Experiment results

For four kinds of query viewpoints, the results are shown in below figure.

<div align="center">
  <img src="https://github.com/sxzrt/The-PersonX-dataset/blob/master/images/test1.jpg" width="400">
</div>


Under each query viewpoint, we report 36 rank-1 scores obtained by the query to retrieve 36 types of true match viewpoints. The mean value of 36 rank-1 scores of each kind of query viewpoint is also reported.

### 3.3.3 Subsection conclusions

* The query viewpoint of left/right generally leads to higher re-ID accuracy than front/back viewpoints.



## 4. Citation

****

If you use this dataset in your research, please kindly cite our work as, <br>

```
@inproceedings{sun2019dissecting,
	title={Dissecting Person Re-identification from the Viewpoint of Viewpoint},
	author={Sun, Xiaoxiao and Zheng, Liang},
	booktitle={CVPR},
	year={2019}
}
```
