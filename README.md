<base target="_blank"/>

# Image_Harmonization_Datasets: iHarmony4


<img src='examples/results_gif.gif' align="center" width=1024>

### Try this [online demo](https://bcmi.sjtu.edu.cn/home/niuli/demo_image_composition/) for image harmonization and have fun!

**Image Harmonization** is to harmonize a composite image by adjusting its foreground appearances consistent with the background region. A real composite image is generated by a foreground region of one image combined with the background of another image. Though it's easy to create real composite images, the harmonized outputs are too time-consuming and skill-demanding to generate. So there is no high-quality publicly available dataset for image harmonization.

Our dataset **iHarmony4** is a synthesized dataset for Image Harmonization. It contains 4 sub-datasets: **HCOCO**,**HAdobe5k**, **HFlickr**, and **Hday2night**, each of which contains synthesized composite images, foreground masks of composite images and corresponding real images. The iHarmony4 dataset is provided in  [**Baidu Cloud**](https://pan.baidu.com/s/1xEN0Xrv_MbuKT0ZqsipeEg) (access code: kqz3) [**(Alternative_address)**](https://cloud.bcmi.sjtu.edu.cn/sharing/CCExSuoDA)

| |HCOCO|HAdobe5k|HFlickr|Hday2night|iHarmony4 |
|:--:|:--:|:--:|:--:|:--:|:--:|
|Training set| 38545 |19437| 7449 |311|65742|
|Test set| 4283 |2160| 828 |133|7404|

### 1. HCOCO

HCOCO, containing 42k synthesized composite images, is generated based on [Microsoft COCO](<http://cocodataset.org/>) dataset. The foreground region is corresponding object segmentation mask provided from COCO. Within the foreground region, the appearance of COCO image is edited using various color transfer methods. **The HCOCO sub-dataset and training/testing split are provided in [Baidu Cloud](https://pan.baidu.com/s/11oXE9fXU1n54JUTxaI6Nzw )**(access code:  ab5e)  [**(Alternative_address1)**](https://cloud.bcmi.sjtu.edu.cn/sharing/iv9v9j2Na) [**(Alternative_address2)**](https://drive.google.com/file/d/1YfC-P94eJ66hWvd5YQKHTKpZ1j3G2j0T/view?usp=sharing).


### 2. HAdobe5k

HAdobe5k is generated based on [MIT-Adobe FiveK](<http://data.csail.mit.edu/graphics/fivek/>) dataset. Provided with 6 editions of the same image, we manually segment the foreground region and exchange foregrounds between 2 versions. **The HAdobe5k sub-dataset and training/testing split are provided in [Baidu Cloud](https://pan.baidu.com/s/1NAtLnCdY1-4uxRKB8REPQg)** [**(Alternative_address)**](https://cloud.bcmi.sjtu.edu.cn/sharing/eBXLV8iU5).


### 3. HFlickr

We collected 4833 images from [Flickr](<https://www.flickr.com/>). After manually segmenting the foreground region, we use the same method as HCOCO to generate HFlickr sub-dataset. **The HFlickr sub-dataset and training/testing split are provided in [Baidu Cloud](https://pan.baidu.com/s/1ZaCYo9Z21RGVgCwXgtvmbw)** [**(Alternative_address)**](https://cloud.bcmi.sjtu.edu.cn/sharing/IJu8Q2fK5).


### 4. Hday2night

Hday2night is generated based on [day2night](https://pan.baidu.com/s/1bCtVhhtb_EDool_UnN2Bjw) dataset. We manually segment the foreground region, which is cropped and overlaid on another image captured on a different time. **The Hday2night sub-dataset and training/testing split are provided in [Baidu Cloud](https://pan.baidu.com/s/1wTqGeB9SMweS5UAaxWof8A)** [**(Alternative_address)**](https://cloud.bcmi.sjtu.edu.cn/sharing/sFqh4GfI9).


![](examples/examples.jpg)

# Color Transfer Methods

To generate synthesized composite images, color transfer methods are adopted to transfer color information from reference images to real images. Considering that color transfer methods can be categorized into four groups based on parametric/non-parametric and correlated/decorrelated color space, we select one representative method from each group. Thanks to Wei Xu's efforts for releasing the code of color transfer method 1, 2 and 3 in their survey paper, we could implement color transfer methods specialized for foreground based on their implementation. And the source code of IDT regrain color transfer is downloaded from the author's [GitHub](https://github.com/frcs/colour-transfer)

### 1. global color transfer

--Parametric method in decorrelated color space. Implementation of paper "*Color transfer between images*" [[pdf]](https://www.cs.tau.ac.il/~turkel/imagepapers/ColorTransfer.pdf).

### 2. global color transfer in RGB color space

--Parametric method in correlated color space. Implementation of paper "*Color transfer in correlated color space*" [[pdf]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.530.2757&rep=rep1&type=pdf).

### 3. cumulative histogram mapping

--Non-parametric method in decorrelated color space. Implementation of paper  ""*Histogram-based prefiltering for luminance and chrominance compensation of multiview video*" [[pdf]](https://ieeexplore.ieee.org/document/4539698).

### 4. IDT regrain color transfer

--Non-parametric method in correlated color space. Implementation of paper  "*Automated colour grading using colour distribution transfer*" [[pdf]](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.458.7694&rep=rep1&type=pdf).



# Our DoveNet

Here we provide PyTorch implementation and the trained model of our DoveNet.

## Prerequisites

- Linux
- Python 3
- CPU or NVIDIA GPU + CUDA CuDNN

## Getting Started

### Installation

- Clone this repo:

```bash
git clone https://github.com/bcmi/Image_Harmonization_Datasets.git
cd Image_Harmonization_Datasets
```

- Download the iHarmony4 dataset.
- Install [PyTorch](http://pytorch.org) 1.2  and other dependencies (e.g., torchvision, [visdom](https://github.com/facebookresearch/visdom) and [dominate](https://github.com/Knio/dominate)).
  - For Conda users,  you can create a new Conda environment using `conda env create -f environment.yaml`.

### DoveNet train/test

- To view training results and loss plots, run `python -m visdom.server` and click the URL http://localhost:8097.
- Train a model:

```bash
#!./scripts/train_dovenet.sh
python train.py  --dataset_root <path_to_iHarmony4_dataset> --name experiment_name  --model dovenet --dataset_mode iharmony4 --is_train 1  --gan_mode wgangp  --norm instance --no_flip --preprocess none --netG s2ad
```

Remember to specify `dataset_root` and `name` in the corresponding place.

To see more intermediate results, you can check out visdom or `./checkpoints/experiment_name/web/index.html`.

- Test the model:

```bash
#!./scripts/test_dovenet.sh
python test.py --dataset_root <path_to_iHarmony4_dataset> --name experiment_name --model dovenet --dataset_mode iharmony4 --netG s2ad --is_train 0  --norm instance --no_flip --preprocess none --num_test 7404
```

Remember to specify `dataset_root` and `name` in the corresponding places.

When testing, it prints the results of evaluation metrics MSE and PSNR. It also saves the harmonized outputs in `./results/experiment_name/latest_test/images/`

### Apply a pre-trained DoveNet model

Our pre-trained model is available on [**Baidu Cloud**](https://pan.baidu.com/s/12oGrBF88O-x0BlWGVkMjag) (access code: 8q8a) [**(Alternative_address)**](https://bcmi.cloud:5001/sharing/WQkB0OnXK). Download and save it at `./checkpoints/experiment_name_pretrain/latest_net_G.pth`. 

As both instance normalization and batch normalization perform well for our task,  **the model we provided here is the one using batch normalization**.

To test its performance on iHarmony4 dataset, using:

```bash
python test.py --dataset_root <path_to_iHarmony4_dataset> --name experiment_name_pretrain --model dovenet --dataset_mode iharmony4 --netG s2ad --is_train 0  --norm batch --no_flip --preprocess none --num_test 7404
```

Note to specify `dataset_root` and `name` in the corresponding place.

# Baselines

Here, we provide the code of baselines used in our paper "**DoveNet: Deep Image Harmonization via Domain Verification**", which is accepted by **CVPR2020**.  Refer to [Bibtex](#Bibtex) for more details.

### 1. Lalonde and Efros

J.-F. Lalonde et al. provides their implementation of paper  "*Using color compatibility for assessing image realism*" (ICCV2017) in their [GitHub](https://github.com/jflalonde/colorRealism).

And we have arranged the code to a "click-and-run" way. 
`demo.m` is available in `/lalonde/colorStatistics/mycode/demo/`. 
Don't forget to specify the path of the code and results in your computer in `getPathName.m`, and run `setPath.m` before run `demo.m`to get everything ready.

### 2. Xue *et al.*

This is Xue's implementation of their paper in 2012 ACM Transactions on Graphics "*Understanding and improving the realism of image composites*".

`demo.m` is available in `/xue/demo/`. 

Notice to add the path of all dependent files using `addpath(genpath('../dependency'))`.

### 3. Zhu *et al.*

Jun-Yan Zhu released the code of their paper "*Learning a discriminative model for the perception of realism in composite images*" (ICCV2015) in their [GitHub](<https://github.com/junyanz/RealismCNN>).

Notice that it requires matcaffe interface. We make some changes corresponds to our dataset including how to preprocess data and how to save the harmonized results. Don't forget to specify `DATA_DIR`,`MODEL_DIR` and `RST_DIR` before running `demo.m`.

The pre-trained models of Zhu's work can also be found in [BaiduCloud](https://pan.baidu.com/s/1_9UidT1rGNX0gYOYzjqITQ) and remember to put it under `MODEL_DIR`.

### 4. DIH

Tsai released their pre-trained caffe model of their paper "*Deep Image Harmonization*" (CVPR2017)  in their [GitHub](<https://github.com/wasidennis/DeepHarmonization>). This is a Tensorflow implementation based on the released caffe network.

Besides, we discard one inner-most convolutional layer and one inner-most deconvolutional layer to make it suitable for input of 256\*256 size. In DIH, they proposed to use segmentation branch to help propogate semantics to harmonization branch and it contributes considerable improvments. So here we inplement this two versions, DIH without  segmentation branch and  DIH with segmentation branch, corresponding to DIH(w/o semantics) and DIH in their paper.

   - #### without  segmentation branch

We discard the scene parsing branch and preserve the remaining encoder-decoder structure and skip links.  And this is the version used as one of the baselines in our paper.

To train DIH(w/o semantics) , under the folder `wo_semantics/`, run: 

`python train.py --data_dir <Your Path to Dataset> --init_lr 0.0001 --batch_size 32`

Don't forget to specify the directory of Image Harmonization Dataset after `data_dir`.

Our trained model can be found in [BaiduCloud](https://pan.baidu.com/s/1Uxf-bxJLh5W_TJbFl_cqOw). To test and re-produce the results, remember to put the model under `/dih/wo_semantics/model/` and run:

`python test.py --batch_size 1`

   - #### with segmentation branch

The structure is implemented the same as the Caffe network. In DIH, to pre-train the joint network, they constructed a synthesized composite dataset based on ADE20K, which provides images segmentations. While  we use HCOCO instead, leveraging existing segmentaitons of COCO images from COCO-Stuff dataset [GitHub](https://github.com/nightrome/cocostuff).  In our experiment, we leverage the object segmentations to pretrain the network. After downloading the dataset, preprocess the PNG segmentation to filter out stuff labels. Remember to rename the corresponding segmentation with the same name as real images and put them under `<Your Path to Dataset>/HCOCO/object_segmentations/`. Then, we freeze the segmentation branch and finetune harmonization branch using the whole dataset. 

To pre-train this model, under the folder `with_semantics/`, run:

`python train_seg.py --data_dir <Your Path to Dataset> --init_lr 0.0001 --batch_size 32`

After that, freeze the segmentation branch and finetune harmonization branch. Run:

`python finetune.py --data_dir <Your Path to Dataset> --init_lr 0.0001 --batch_size 32 --pretrain False`

Specify the directory of Image Harmonization Dataset after `data_dir`.

Our trained model can be found in [BaiduCloud](https://pan.baidu.com/s/1mUCNsT-3yAN6JHoPiNp9Xg). To test and re-produce the results, remember to put the model under `/dih/with_semantics/model/` and run:

`python test_seg.py --batch_size 1`

### 5. S<sup>2</sup>AM

Cun and Pan released the code and model of their paper "*Improving the Harmony of the Composite Image by Spatial-Separated Attention Module*" (TIP2020)  in [GitHub](https://github.com/vinthony/s2am). They provide the [model](https://drive.google.com/file/d/1bm1ZdZ4xmV9fKCQBDsulvYwrxPAidZ3T/view?usp=sharing) trained on their SCOCO and S-Adobe5k dataset and the [models](https://uofmacau-my.sharepoint.com/:f:/g/personal/yb87432_umac_mo/EpemCJwfnhpIoDNAMfiegqIB0RXkdKH9Z2WibJJ4s27PbA?e=qPNzpI) trained on each sub-dataset of iHarmony4 individually. To facilitate the fair comparison, we adopt the same training strategy to train S<sup>2</sup>AM model on the merged training set of four sub-datasets using the released code from their GitHub. The trained model could be found in [BaiduCloud](https://pan.baidu.com/s/1xTlNxjX9l0WJxx4I5d4zgA) (access code: 92tj).

### 6. HRNet

The released code and model of paper "*Object-Contextual Representations for Semantic Segmentation*" (ECCV2020) could be found in [GitHub](<https://github.com/HRNet/HRNet-Semantic-Segmentation>). We transplant the code to make it suitable for image harmonization task and adopt the default training strategy to train it from scratch. The trained model and test code could be found in [BaiduCloud]() (access code: ).

### 7. SSH

Yifan Jiang provide the inference code and pretrained weight of their paper "*SSH: A Self-Supervised Framework for Image Harmonization*" (ICCV2021) in their [GitHub](<https://github.com/VITA-Group/SSHarmonization>).
Notice that SSH has different input format (separate foreground and background image) and training data (image crops processed with LUTs), which does not strictly match our setting. 
Here we directly testing the released model from the official SSH [GitHub](https://github.com/VITA-Group/SSHarmonization), in which the composite image is regarded as the input in the testing pipeline of SSH, while the ground-truth real image is the reference. We make some changes corresponds to our dataset in `demo.ipynb` and convert it to `test.py`, which could be found in [BaiduCloud](https://pan.baidu.com/s/12WjdMhOODx68tzGFgZhaSw ) (access code: 2tdg). The test images are resized to 256\*256 for for fair comparison.

To test and re-produce the results, remember to put the pretrained weight downloaded from the official SSH [GitHub](https://github.com/VITA-Group/SSHarmonization) under `./`, modify the data paths in `test.py`, and run:

`python test.py`




# Experiments

When conducting experiments, we merge training sets of four sub-datasets as a whole training set to train the model, and evaluate it on the test set of each sub-dataset and the whole test set. Here we show the results of recent baselines on our iHarmony4 dataset based on MSE and PSNR metrics. In addition, we also provide the fMSE (foreground MSE) score on the whole test set to facilitate future study. The paper/code/model of image harmonization related methods are summarized in [Awesome-image-harmonization](https://github.com/bcmi/Awesome-Image-Harmonization). **The following leaderboard is based on fMSE metric.**

<table class="tg">
  <tr>
    <th class="tg-0pky" align="center">Sub-dataset</th>
    <th class="tg-0pky" rowspan="2" align="center">Extra info</th>
    <th class="tg-0pky" colspan="3" align="center">All</th>
    <th class="tg-0pky" colspan="2" align="center">HCOCO</th>
    <th class="tg-0pky" colspan="2" align="center">HAdobe5k</th>
    <th class="tg-0pky" colspan="2" align="center">HFlickr</th>
    <th class="tg-0pky" colspan="2" align="center">Hday2night</th>  
  </tr>
  <tr>
    <th class="tg-0pky" align="center">Evaluation metric</th>
    <th class="tg-0pky" align="center">fMSE</th>
    <th class="tg-0pky" align="center">MSE</th>
    <th class="tg-0pky" align="center">PSNR</th>    
    <th class="tg-0pky" align="center">MSE</th>
    <th class="tg-0pky" align="center">PSNR</th>
    <th class="tg-0pky" align="center">MSE</th>
    <th class="tg-0pky" align="center">PSNR</th>
    <th class="tg-0pky" align="center">MSE</th>
    <th class="tg-0pky" align="center">PSNR</th>
    <th class="tg-0pky" align="center">MSE</th>
    <th class="tg-0pky" align="center">PSNR</th>    
  </tr>
  <tr>
    <td class="tg-0pky" align="center">input composite</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">1387.30</td>
    <td class="tg-0pky" align="center">172.47</td>
    <td class="tg-0pky" align="center">31.63</td>
    <td class="tg-0pky" align="center">69.37</td>
    <td class="tg-0pky" align="center">33.94</td>
    <td class="tg-0pky" align="center">345.54</td>
    <td class="tg-0pky" align="center">28.16</td>
    <td class="tg-0pky" align="center">264.35</td>
    <td class="tg-0pky" align="center">28.32</td>
    <td class="tg-0pky" align="center">109.65</td>
    <td class="tg-0pky" align="center">34.01</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://openaccess.thecvf.com/content/WACV2021/papers/Sofiiuk_Foreground-Aware_Semantic_Representations_for_Image_Harmonization_WACV_2021_paper.pdf">iSSAM</a> [WACV2021]<sup>@</sup></td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">264.96</td>
    <td class="tg-0pky" align="center">24.44</td>
    <td class="tg-0pky" align="center">38.19</td>
    <td class="tg-0pky" align="center">16.48</td>
    <td class="tg-0pky" align="center">39.16</td>
    <td class="tg-0pky" align="center">21.88</td>
    <td class="tg-0pky" align="center">38.08</td>
    <td class="tg-0pky" align="center">69.67</td>
    <td class="tg-0pky" align="center">33.56</td>
    <td class="tg-0pky" align="center">40.59</td>
    <td class="tg-0pky" align="center">37.72</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://openaccess.thecvf.com/content/ICCV2021/papers/Guo_Image_Harmonization_With_Transformer_ICCV_2021_paper.pdf">D-HT</a> [ICCV2021]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">320.78</td>
    <td class="tg-0pky" align="center">30.30</td>
    <td class="tg-0pky" align="center">37.55</td>
    <td class="tg-0pky" align="center">16.89</td>
    <td class="tg-0pky" align="center">38.76</td>
    <td class="tg-0pky" align="center">38.53</td>
    <td class="tg-0pky" align="center">36.88</td>
    <td class="tg-0pky" align="center">74.51</td>
    <td class="tg-0pky" align="center">33.13</td>
    <td class="tg-0pky" align="center">53.01</td>
    <td class="tg-0pky" align="center">37.10</td>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="2" align="center"><a href="https://openaccess.thecvf.com/content/WACV2021/papers/Sofiiuk_Foreground-Aware_Semantic_Representations_for_Image_Harmonization_WACV_2021_paper.pdf">iDIH</a> [WACV2021]</td>
    <td class="tg-0pky" align="center">-<sup>#</sup></td>
    <td class="tg-0pky" align="center">341.77</td>
    <td class="tg-0pky" align="center">31.71</td>
    <td class="tg-0pky" align="center">37.14</td>
    <td class="tg-0pky" align="center">19.51</td>
    <td class="tg-0pky" align="center">38.40</td>
    <td class="tg-0pky" align="center">33.81</td>
    <td class="tg-0pky" align="center">36.39</td>
    <td class="tg-0pky" align="center">86.44</td>
    <td class="tg-0pky" align="center">32.60</td>
    <td class="tg-0pky" align="center">49.94</td>
    <td class="tg-0pky" align="center">37.01</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center">S</td>
    <td class="tg-0pky" align="center">252.00</td>
    <td class="tg-0pky" align="center">22.00</td>
    <td class="tg-0pky" align="center">38.31</td>
    <td class="tg-0pky" align="center">14.01</td>
    <td class="tg-0pky" align="center">39.64</td>
    <td class="tg-0pky" align="center">21.36</td>
    <td class="tg-0pky" align="center">37.35</td>
    <td class="tg-0pky" align="center">60.41</td>
    <td class="tg-0pky" align="center">34.03</td>
    <td class="tg-0pky" align="center">50.61</td>
    <td class="tg-0pky" align="center">37.68</td>
  </tr> 
  <tr>
    <td class="tg-0pky" align="center"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Guo_Intrinsic_Image_Harmonization_CVPR_2021_paper.pdf">Guo et al.</a> [CVPR2021]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">400.29</td>
    <td class="tg-0pky" align="center">38.71</td>
    <td class="tg-0pky" align="center">35.90</td>
    <td class="tg-0pky" align="center">24.92</td>
    <td class="tg-0pky" align="center">37.16</td>
    <td class="tg-0pky" align="center">43.02</td>
    <td class="tg-0pky" align="center">35.20</td>
    <td class="tg-0pky" align="center">105.13</td>
    <td class="tg-0pky" align="center">31.34</td>
    <td class="tg-0pky" align="center">55.53</td>
    <td class="tg-0pky" align="center">35.96</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://arxiv.org/pdf/2009.09169.pdf">BargainNet</a> [ICME2021]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">405.23</td>
    <td class="tg-0pky" align="center">37.82</td>
    <td class="tg-0pky" align="center">35.88</td>
    <td class="tg-0pky" align="center">24.84</td>
    <td class="tg-0pky" align="center">37.03</td>
    <td class="tg-0pky" align="center">39.94</td>
    <td class="tg-0pky" align="center">35.34</td>
    <td class="tg-0pky" align="center">97.32</td>
    <td class="tg-0pky" align="center">31.34</td>
    <td class="tg-0pky" align="center">50.98</td>
    <td class="tg-0pky" align="center">35.67</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://www.bmvc2020-conference.com/assets/papers/0121.pdf">Hao et al.</a> [BMVC2020]<sup>+</sup></td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">437.90</td>
    <td class="tg-0pky" align="center">38.46</td>
    <td class="tg-0pky" align="center">35.91</td>
    <td class="tg-0pky" align="center">23.44</td>
    <td class="tg-0pky" align="center">37.33</td>
    <td class="tg-0pky" align="center">39.22</td>
    <td class="tg-0pky" align="center">34.80</td>
    <td class="tg-0pky" align="center">112.39</td>
    <td class="tg-0pky" align="center">31.29</td>
    <td class="tg-0pky" align="center">49.73</td>
    <td class="tg-0pky" align="center">36.96</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://openaccess.thecvf.com/content/CVPR2021/papers/Ling_Region-Aware_Adaptive_Instance_Normalization_for_Image_Harmonization_CVPR_2021_paper.pdf">RainNet</a> [CVPR2021]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">469.60</td>
    <td class="tg-0pky" align="center">40.29</td>
    <td class="tg-0pky" align="center">36.12</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">37.08</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">36.22</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">31.64</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">34.83</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://arxiv.org/pdf/1907.06406.pdf">S<sup>2</sup>AM</a> [TIP2020]<sup>*</sup></td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">481.79</td>
    <td class="tg-0pky" align="center">48.00</td>
    <td class="tg-0pky" align="center">35.29</td>
    <td class="tg-0pky" align="center">33.07</td>
    <td class="tg-0pky" align="center">36.09</td>
    <td class="tg-0pky" align="center">48.22</td>
    <td class="tg-0pky" align="center">35.34</td>
    <td class="tg-0pky" align="center">124.53</td>
    <td class="tg-0pky" align="center">31.00</td>
    <td class="tg-0pky" align="center">48.78</td>
    <td class="tg-0pky" align="center">35.60</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://openaccess.thecvf.com/content_CVPR_2020/papers/Cong_DoveNet_Deep_Image_Harmonization_via_Domain_Verification_CVPR_2020_paper.pdf">DoveNet</a> [CVPR2020]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">549.96</td>
    <td class="tg-0pky" align="center">52.36</td>
    <td class="tg-0pky" align="center">34.75</td>
    <td class="tg-0pky" align="center">36.72</td>
    <td class="tg-0pky" align="center">35.83</td>
    <td class="tg-0pky" align="center">52.32</td>
    <td class="tg-0pky" align="center">34.34</td>
    <td class="tg-0pky" align="center">133.14</td>
    <td class="tg-0pky" align="center">30.21</td>
    <td class="tg-0pky" align="center">54.05</td>
    <td class="tg-0pky" align="center">35.18</td>
  </tr>
  <tr>
    <td class="tg-0pky" rowspan="2" align="center"><a href="https://openaccess.thecvf.com/content_cvpr_2017/papers/Tsai_Deep_Image_Harmonization_CVPR_2017_paper.pdf">DIH</a> [CVPR2017]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">773.18</td>
    <td class="tg-0pky" align="center">76.77</td>
    <td class="tg-0pky" align="center">33.41</td> 
    <td class="tg-0pky" align="center">51.85</td>
    <td class="tg-0pky" align="center">34.69</td>
    <td class="tg-0pky" align="center">92.65</td>
    <td class="tg-0pky" align="center">32.28</td>
    <td class="tg-0pky" align="center">163.38</td>
    <td class="tg-0pky" align="center">29.55</td>
    <td class="tg-0pky" align="center">82.34</td>
    <td class="tg-0pky" align="center">34.62</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center">S</td>
    <td class="tg-0pky" align="center">769.79</td>
    <td class="tg-0pky" align="center">76.63</td>
    <td class="tg-0pky" align="center">33.50</td>
    <td class="tg-0pky" align="center">49.63</td>
    <td class="tg-0pky" align="center">34.80</td>
    <td class="tg-0pky" align="center">95.41</td>
    <td class="tg-0pky" align="center">32.29</td>
    <td class="tg-0pky" align="center">168.62</td>
    <td class="tg-0pky" align="center">29.58</td>
    <td class="tg-0pky" align="center">68.81</td>
    <td class="tg-0pky" align="center">35.51</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://arxiv.org/pdf/2108.06805.pdf">SSH</a> [ICCV2021]<sup>%</sup></td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">1140.66</td>
    <td class="tg-0pky" align="center">89.23</td>
    <td class="tg-0pky" align="center">32.77</td>   
    <td class="tg-0pky" align="center">73.20</td>
    <td class="tg-0pky" align="center">34.03</td>
    <td class="tg-0pky" align="center">115.03</td>
    <td class="tg-0pky" align="center">31.73</td>
    <td class="tg-0pky" align="center">266.56</td>
    <td class="tg-0pky" align="center">28.68</td>
    <td class="tg-0pky" align="center">98.84</td>
    <td class="tg-0pky" align="center">34.56</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://graphics.cs.yale.edu/sites/default/files/2012sig_compositing.pdf">Xue et al.</a> [TOG2012]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">1411.40</td>
    <td class="tg-0pky" align="center">155.87</td>
    <td class="tg-0pky" align="center">31.40</td>   
    <td class="tg-0pky" align="center">77.04</td>
    <td class="tg-0pky" align="center">33.32</td>
    <td class="tg-0pky" align="center">274.15</td>
    <td class="tg-0pky" align="center">28.79</td>
    <td class="tg-0pky" align="center">249.54</td>
    <td class="tg-0pky" align="center">28.32</td>
    <td class="tg-0pky" align="center">190.51</td>
    <td class="tg-0pky" align="center">31.24</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="http://vision.gel.ulaval.ca/~jflalonde/assets/pubs/papers/lalonde_iccv_07.pdf">Lalonde and Efros</a> [ICCV2017]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">1433.21</td>
    <td class="tg-0pky" align="center">150.53</td>
    <td class="tg-0pky" align="center">30.16</td>
    <td class="tg-0pky" align="center">110.10</td>
    <td class="tg-0pky" align="center">31.14</td>
    <td class="tg-0pky" align="center">158.90</td>
    <td class="tg-0pky" align="center">29.66</td>
    <td class="tg-0pky" align="center">329.87</td>
    <td class="tg-0pky" align="center">26.43</td>
    <td class="tg-0pky" align="center">199.93</td>
    <td class="tg-0pky" align="center">29.80</td>
  </tr>
  <tr>
    <td class="tg-0pky" align="center"><a href="https://www.cv-foundation.org/openaccess/content_iccv_2015/papers/Zhu_Learning_a_Discriminative_ICCV_2015_paper.pdf">Zhu et al.</a> [ICCV2015]</td>
    <td class="tg-0pky" align="center">-</td>
    <td class="tg-0pky" align="center">1580.17</td>
    <td class="tg-0pky" align="center">204.77</td>
    <td class="tg-0pky" align="center">30.72</td>
    <td class="tg-0pky" align="center">79.82</td>
    <td class="tg-0pky" align="center">33.04</td>
    <td class="tg-0pky" align="center">414.31</td>
    <td class="tg-0pky" align="center">27.26</td>
    <td class="tg-0pky" align="center">315.42</td>
    <td class="tg-0pky" align="center">27.52</td>
    <td class="tg-0pky" align="center">136.71</td>
    <td class="tg-0pky" align="center">32.32</td>
  </tr>
</table>








<font size=2>**S** in Extro info indicates using **auxiliary semantic information** in image harmonization.</font>

<font size=2.5>**\***: Results of S<sup>2</sup>AM here are trained from scratch using the code from the official S<sup>2</sup>AM [GitHub](https://github.com/vinthony/s2am). In the GitHub, they provide results trained on each sub-dataset individually, which we do not include here for fair comparison.</font>

<font size=2.5>**-**: Results of HRNet are trained from scratch using the code from the official HRNet-Semantic-Segmentation [GitHub](https://github.com/HRNet/HRNet-Semantic-Segmentation).</font>

<font size=2>**\+**: Results of Hao et al. here are tested using the released model from the official [GitHub](https://github.com/Dominoer/bmvc2020_image_harmonization) since the results of their released model are not consistent with the reported results in their paper.</font>

<font size=2>**\#**: Results of iDIH backbone without auxiliary semantic information are tested using the released model from the official [GitHub](https://github.com/saic-vul/image_harmonization) since they do not report the detailed results on each sub-dataset in their paper.</font>

<font size=2>**@**: Note that iDIH and iSSAM listed in the table are two different backbones mentioned in the same [paper](https://openaccess.thecvf.com/content/WACV2021/papers/Sofiiuk_Foreground-Aware_Semantic_Representations_for_Image_Harmonization_WACV_2021_paper.pdf) [WACV2021]. Results of iSSAM backbone without auxiliary semantic information are also tested using the released model from the official [GitHub](https://github.com/saic-vul/image_harmonization) since they do not report the fMSE metric in their paper.</font>

<font size=2>**%**: Note that SSH has different input format (separate foreground and background image) and training data (image crops processed with LUTs), which does not strictly match our setting. Results here are tested using the released model from the official SSH [GitHub](https://github.com/VITA-Group/SSHarmonization). The composite image is regarded as the input in the testing pipeline of SSH, while the ground-truth real image is the reference.</font>

<font size=2>Other results without any specifications are directly copied from our DoveNet or other published papers. </font>





Here we also show some example results of different baselines on our dataset. More examples can be found in our main paper.

![](examples/results_syn.jpg)

Besides, to evaluate the effectiveness of different methods in real scenarios, we also conduct user study on 99 real composite images, of which 48 images from Xue and 51 images from Tsai. Below we present several results of different baselines on real composite images. The 99 real composite images could be found in [BaiduCloud](https://pan.baidu.com/s/1z0fvp28gWswSCysw0eiUQg). And to visualize the comparison, we have put the results of different methods on all 99 real composite images in Supplementary.

![](examples/results_real_comp.jpg)

# Other Resources

+ [Awesome-Image-Harmonization](https://github.com/bcmi/Awesome-Image-Harmonization)
+ [Awesome-Image-Composition](https://github.com/bcmi/Awesome-Image-Composition)

# Bibtex

**When using images from our dataset, please cite our paper using the following BibTeX  [[pdf](https://openaccess.thecvf.com/content_CVPR_2020/papers/Cong_DoveNet_Deep_Image_Harmonization_via_Domain_Verification_CVPR_2020_paper.pdf)] [[supp](https://openaccess.thecvf.com/content_CVPR_2020/supplemental/Cong_DoveNet_Deep_Image_CVPR_2020_supplemental.pdf)] [[arxiv](https://arxiv.org/abs/1911.13239)]:**

```
@inproceedings{DoveNet2020,
title={DoveNet: Deep Image Harmonization via Domain Verification},
author={Wenyan Cong and Jianfu Zhang and Li Niu and Liu Liu and Zhixin Ling and Weiyuan Li and Liqing Zhang},
booktitle={CVPR},
year={2020}}
```

