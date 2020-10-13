
# A generalized tool for accurate and efficient multi-lens multispectral camera image registration


Due to multi-lens camera utilizes each lens to record separate heterogeneous images, such as visible (RGB), multispectral (MS), or thermal (TIR) information, the differences of viewpoints and lens distortion leading to significant ghost effects of original images. To recover one-sensor geometry for precise spectral measurements, it requires band co-registration processing to correct the misregistration errors (MREs).

Based on our years research, we have developed a generalized tool, named **Robust and Adaptive Band-to-Band Image Transform (RABBIT)**, that can accuratly correct the MREs of various state-of-the-art multi-lens cameras. 
 
MREs of Original Image | Results of RABBIT
:------------: | :-------------:
 ![](/Images/Altum_MRE.png) |![](/Images/Altum_Co.png) 
 
<!--more-->

## Supported Multi-lens Camera 
RABBIT supports various state-of-the-art multi-lens structured cameras as listed below.<br>

Manufacture|Tetracam | Micasense Rededge | Parrot 
:------------: |:------------: | :-------------: | :------------: 
 Camera|![](/Images/MCA_12.png) |![](/Images/Altum.png)|![](/Images/Sequoia.png) 
 Models|MCA-4, MCA-6, MCA-12|MX, Altum|Sequoia|Duo Pro R
 Multispectral| BLU, GRE, RED, REG, NIR|BLU, GRE, RED, REG, NIR| GRE, RED, REG, NIR|RGB + TIR

**Please contact us if you need a different model.*
## Features
### 1. Sucessful Image Matching
 RABBIT utilizes a novel **N-SURF** matching that can extract more features and increase the amount of correct matches on heterogenous images. This is a crucial step to connect images and estimnate the image transformation coefficients.
 

### 2. Proper Image Transform
  RABBIT adopts an **extended projective transform (EPT)** that can correct the differences of viewpoints and lens distortion effects. Compared to **affine transform (AT)** and **projective transform (PT)**, EPT has the smallest residuals of MREs and best accuracy of co-registration results.
  
	AT: Only parallel image rotation and translation are considered.
	PT: Considering three dimensions axises rotation and translation.
	EPT: Simliar to PT, but the lens distortion differences are also considered.

AT| PT| EPT
:------------: | :-------------: | :-------------:
 ![](/Images/AT.png) |![](/Images/PT.png)|![](/Images/EPT.png)
### 3. High Accuracy
 RABBIT can achieve 0.2-0.7 pixels band co-registration accuracy.
 
MCA-12| Altum| Sequoia
:------------: | :-------------: | :-------------:
![](/Images/Accuracy_MCA.png)|![](/Images/Accuracy_Altum.png)|![](/Images/Accuracy_Sequoia.png)
### 4. High Efficiency
RABBIT can run under multi-thread CPU or GPU. The comparisons of processing efficiency are listed in the following table.	

	CPU: Intel(R) Core(TM) i7-8700 3.2 GHz
	GPU: NVIDIA Geforce GTX TITAN X   
    
Camera|Tetracam MCA-12| Micasense Altum |Parrot Sequoia 
:------------: | :-------------: | :-------------:| :-------------:
Image Resolution| 1280 X 1024 | 2064 X 1544 | 1280 X 960 
 Bands X Groups|12 X 100|5 X 100|4 X 100
 Independent in Multi-thread CPU|65 min.|62 min.|17 min.
 Independent in GPU|13 min.|14 min.|3 min.
 Batch in Multi-thread CPU|2 min.|2 min.|2 min.
 Batch in GPU|2 min.|2 min.|2 min.
 
### 4. Results
Camera|MCA-12 | Altum| Sequoia 
:------------: |:------------: | :-------------: | :------------: 
 Original|![](/Images/MCA_Original.gif) |![](/Images/Altum_Original.gif)|![](/Images/Sequoia_Original.gif) 
 RABBIT |![](/Images/MCA_RABBIT.gif) |![](/Images/Altum_RABBIT.gif)|![](/Images/Sequoia_RABBIT.gif) 

## How to Use
RABBIT is easy to use within three simple steps.
### 1. Create a new project and chose the camera model.
### 2. Select the image path of ordered folders.
For example of Micasense RedEdge Altum:
.../Images
....../Mica1
....../Mica2
....../Mica3
....../Mica4
....../Mica5

***2.1 Read Camera Information (FLIR Duo Pro R Only)***
Since the sensors of FLIR has different image resolutions and focal lengths, it requires to import the interior orientation parameters in advance.

### 3. Run the RABBIT without modifiying the settings.
All images are loded and only master bands are shown on the left pannel. Click the **Run** buttom below and your results will sotre in the RABBIT folder. 

Chose Camera Model | Select Image Patch | Run RABBIT
:------------: | :-------------: | :------------:
 ![](/Images/NewPorject_1.png)|![](/Images/NewPorject_2.png)|![](/Images/Run.png) 
### 4. Settings<br>
**4.1 Feature Points**
  The default value is 2% of the image resolution. Change to a higher or lower value to increase matching reliablity or reduce processing time. However, we do not recommend adjusting it, because a higher value of feature points will not siginficantly affect the processing time in GPU mode, but it does in CPU mode.
  
**4.2 Independent vs. Batch**
**Independent** is to conduct N-SURF matching and MPT on each group of images, which can obtain more accurate and reliable reuslts.
**Batch** is to use same coefficients from one corrected group of images, which can achive ultra-speed processing efficiency. 

***When to use batch mode***
If all lenses of camera are highly synchronized, have same shutter speed, and the observation object distances are constant, we can treat the imaging geometry among all groups of images are the same. Therefore, we can simply choose one group of image for correction and then apply the obtained coefficients to the rest groups of images. 

** Tips: Choose one group of images that have rich texture and high contrast for correction.

**4.3 CPU vs. GPU**
By default, RABBIT detects your most poweful GPU device for increasing the processing efficincy. 

We use **Alea GPU commuity** for GPU accelerating, see what kinds of GPU cards are supported in [Alea GPU](http://www.aleagpu.com/release/3_0_4/doc/license.html)

Setting | Accuracy of one group
:------------: | :-------------: | 
 ![](/Images/Setting_1.png) |![](/Images/Setting_2.png)|
## Publications 
If RABBIT help in your research/projects, please consider citing the following papers. 

[Jhan, J.-P., Rau, J.-Y., Huang, C.-Y., 2016. Band-to-band registration and ortho-rectification of multilens/multispectral imagery: A case study of MiniMCA-12 acquired by a fixed-wing UAS. ISPRS Journal of Photogrammetry and Remote Sensing 114, 66-77.](http://dx.doi.org/10.1016/j.isprsjprs.2016.01.008)

[Jhan, J.-P., Rau, J.-Y., Haala, N., 2018. Robust and adaptive band-to-band image transform of UAS miniature multi-lens multispectral camera. ISPRS Journal of Photogrammetry and Remote Sensing 137, 47-60.](https://doi.org/10.1016/j.isprsjprs.2017.12.009)

[Jhan, J.-P., Rau, J.-Y., 2020. A generalized tool for accurate and efficient multi-lens multispectral camera image registration. IEEE Transactions of Geoscience and Remote Sensing, under review]()

## Download

[RABBIT](https://github.com/jpjhan/RABBIT/tree/master)
[Sample Dataset](https://reurl.cc/WdevD9) 

## Contacts
If you have any questions about RABBIT, please contact Dr. Jyun-Ping Jhan
mailto:jyunpingjhan@geomatics.ncku.edu.tw  

