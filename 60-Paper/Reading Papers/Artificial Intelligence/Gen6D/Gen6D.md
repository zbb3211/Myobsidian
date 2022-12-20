# Gen6D: Generalizable Model-Free 6-DoF Object Pose Estimation from RGB Images
**Yuan Liu, Yilin Wen, Sida Peng, Cheng Lin, Xiaoxiao Long, Taku Komura, and Wenping Wang**
**Keywords:6-DoF Object Pose Estimation; Camera Pose Estimation**
---
## 一、总结、评价和应用
## 二、 文章的主要问题
## 三、结论
## 四、思想脉络
### 1 Introduction
The framework consists of an object detector, a viewpoint selector and a pose refiner. Given reference images and a query image, an *object detector* first detects the object regions by correlating the reference images with the query image. Then, a *viewpoint selector* matches the query image against the reference images to produce a coarse initial pose. Finally, the initial pose is further refined by a *pose refiner* to search for an accurate object pose.
- A challenge is **how to design a viewpoint selector** when the reference images are sparse and contain cluttered background.
- The main challenge of **developing our pose refiner** is the unavailability of the object model.
### 2 Related works
#### 2.1 Specific object pose estimator
Most object pose estimatorsare instance-specific, which cannot generalize to unseen objects and usually require a 3D model of the object to render extensive images for training. In comparison, Gen6D is generalizable, which makes no assumption of the category or the instance of the object and also does not need the 3D model of the object.
#### 2.2 Generalizable object pose estimator
Current rendering methods are only able to render images under exactly the same appearance like lighting conditions, which *degenerates the accuracy under varying appearance*.
- To remedy this, these methods have to resort to additional *depth maps or object masks* to achieve robustness.
Some works focusing on estimating poses of unseen objects using RGBD sequences.

In contrast to these methods, Gen6D is model-free and does not require depth maps or masks.
#### 2.3 Instance detection
Instance detection aims to detect a given object with some images of the object.
The detector of Gen6D is inspired by `Target driven instance detection.  
34-Deep template-based object instance detection.`, which uses correlation to find the object region. 

The target of Gen6D is *to estimate the 6-DoF object pose*, which is different from these methods for detection or category-level viewpoint estimation.
### 3 Method
Transform the object coordinate to the camera coordinate.
- **Data normalization**: We can estimate a rough size of the object by *triangulating points from reference images or simply unprojecting reference images to find an intersection*. The center of triangulated points or the center of the 3D intersection region is regarded as the **object center**. Then, the *object coordinate system is normalized* so that the object center locates at the origin and the object size is 1, which means the whole object resides inside a unit sphere at the origin.
	- This data normalization ensures the feature volume constructed by our pose refiner in Pose refinement will contain the target object.
- **Overview**: The proposed Gen6D pose estimator consists of ***an object detector, a view selector and a pose refiner***.
	- **The object detector** *crops the object region and estimates an initial translation*(Sec 3.1).
	- **The view selector** *finds an initial rotation* by selecting the most similar reference image and estimating an in-plane rotation(Sec 3.2).
	- The initial translation and rotation are used in the **pose refiner** to *iteratively estimate an accurate pose*(Sec 3.3).
#### 3.1 Detection
***Problem***: The query image is usually very large and the object only occupies a small region on the query image.
***Method***: To focus on the object, we apply a correlation-based instance detector similar to `Target driven instance detection`.
- Finding the 2D projection $q$ of the center.
- Estimating a compact square bounding box size $S_{q}$ that encloses the unit sphere.
	- $S_{q}$ is used in computing the *depth of the object center* by $d = \frac{2\widetilde{f}}{S_{q}}$, where 2 is the *diameter* of the unit sphere and $\widetilde{f}$  is a *virtual focal length* by changing the principle point to the estimated projection $q$.
	- The projection $q$ and the depth $d$ will *determine the location of the object center*, which provides an initial translation for the object pose.
	- ![[capture_20221219142221187.bmp]]

***Design***: The design of our detector as follow:
- ![[capture_20221219142945406.bmp]]
- We *extract feature maps* on both the reference images and the query image by a extract feature maps on both the reference images and the query image by a **VGG network**.
- Then, *the feature maps* of all reference images *are regarded as convolution kernels* to convolve with the feature map of the query image to *get score maps*.
- *To account for scale differences*, we conduct such convolution at $N_{s}$ predefined scales by *resizing the query images to different scales*.
- Based on the multi-scale score maps, we regress *a heat map and a scale map*.
- We select the *location with the max value on the heat map* as the **2D projection of the object center** and use *the scale value $s$ at the same location on the scale map* to **compute the bounding box size** $S_{q}=S_{r}*s$ , where $S_{r}$ is the size of reference images.

***Next***: With the detected *2D projections* and *scales*, we **compute the initial 3D translations and crop the object regions** for subsequent processing.
#### 3.2 Viewpoint Selection
#### 3.3 Pose refinement
#### 3.4 Implementation details
### 4 Experiments
#### 4.1 GenMOP Dataset
We collect a dataset called General Model-free Object Pose Dataset (GenMOP). GenMOP dataset consists of 10 objects ranging from flatten objects like “scissors” to thin structure objects like “chair”. The first 5 objects are used in test and the last 5 objects are used in training.
![[capture_20221219154109110.bmp]]
For each object, two video sequences of the same object are collected in *different environments like backgrounds and lighting conditions*.
Every video sequence is split into ∼200 images. For each sequence, we apply **COLMAP** to *reconstruct the camera poses* in each sequences separately and manually label keypoints on the object for cross-sequence alignment.
#### 4.2 Protocol
We evaluate Gen6D pose estimator on the GenMOP dataset, the LINEMOD dataset and the MOPED dataset.
**GenMOP. LINEMOD. MOPED. Training datasets. Metrics.**
#### 4.3 Results on GenMOP
#### 4.4 Results on LINEMOD
#### 4.5 Results on MOPED
#### 4.6 Analysis
### 5 Conclusion
### Supplementary Materials
#### A Overview
#### B Implementation detail
##### B.1 Data normalization
- **Object size and center.** On the GenMOP dataset, *the object size and object center* are roughly estimated from reconstructed points of COLMAP. On the LINEMOD dataset and the MOPED dataset, we directly use the provided 3D model to compute the object size and the object center. Note we do not need an exact object size and an object center.
	- **Normalization of object coordinate system.** Given the object size $d$ and the object center $c$, we normalize the object coordinate system by $x_{norm}=\frac{(x-c)}{d \times 2}$, where $x_{norm}$ is the normalized coordinate and $x$ is the original coordinate.
	- **Normalization of reference images.** Since the raw reference images *may not look at the object*, we normalize these reference images by *warping the image with a homography and changing the intrinsic correspondingly*. **Note** we can do this normalization because we have already normalized the object coordinate and we know the poses of reference images.
	![[capture_20221219190754925.bmp]]
##### B.2 Detector
**Networks architecture.** The detailed architecture is shown as follow. 
![[capture_20221219205545817.bmp]]
The input reference images are resized to 120 × 120. We actually use *three levels of feature maps* from the query image and *the corresponding three levels of Conv kernels* from reference images. We conduct convolutions on the feature map at each level with the corresponding conv kernel at the same level. $N_{s}=5$ scales are used and we downsample the input query image with $\sqrt 2$ as the factor.

**Loss.** Given the heat map $H$, we regard *all values of the heat map as logits* and use *a binary classification loss* to supervise the heat map prediction. Here, we first project the object center to the image using the ground-truth object pose. Then, a pixel on the heat map is assumed to be correct if it is within one-pixel distance from the object center projection. Otherwise, it is incorrect.
$$\ell_{\text {heat }}=\sum_{p}-\mathbb{1}\left(\left\|p-c_{p r j}\right\|_{2}<1\right) \log \sigma(H(p))-\left(1-\mathbb{1}\left(\left\|p-c_{p r j}\right\|_{2}<1\right)\right) \log (1-\sigma(H(p)))$$
where $\mathbb{1}$ is *an indicator function*, $c_{p r j} \in \mathbb{R}^{2}$ is *the 2D projection of the object center*, $p$ is *a pixel on the heat map*, $\sigma$ is *the Sigmoid function*, $H(p)$ means *the heat value on the pixel $p$*. To supervise the scale map prediction, we apply a **scale loss** to *minimize the L2 distance* between the ground-truth scale and the predicted scale in the log space. Note we only apply such scale loss on the pixels within 1-pixel distance away from the projected object center.
$$\ell_{\text {scale }}=\sum_{p} \mathbb{1}\left(\left\|p-c_{2 d}\right\|_{2}<1\right) \| \log s_{ gt }-S(p) \|_{2}^{2} \text {, }$$
where $S_{gt}$ is is *the ground-truth scale*, $S$ is *the scale map*, $S(p)$ means *the scale value at pixel $p$*. $S_{gt}$ can be computed from the distance $l$ between the camera center and the object center by $S_{gt}=\frac{2f}{lS_{r}}$, where $f$ is *a virtual focal length* by changing the principle point to the object center projection $c_{prj}$, 2 is *the diameter of the unit sphere* and $S_r=120$  is *the size of reference image*.

##### B.3 Selector
##### B.4 Refiner

