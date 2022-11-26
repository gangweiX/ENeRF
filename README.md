# ENeRF: Efficient Neural Radiance Fields for Interactive Free-viewpoint Video

> [Efficient Neural Radiance Fields for Interactive Free-viewpoint Video](https://arxiv.org/abs/2112.01517)  
> Haotong Lin, Sida Peng, Zhen Xu, Yunzhi Yan, Qing Shuai, Hujun Bao and Xiaowei Zhou \
> SIGGRAPH Asia 2022 conference track  
> [Project Page](https://zju3dv.github.io/enerf)

## Installation

### Set up the python environment

```
conda create -n enerf python=3.8
conda activate enerf
pip install torch==1.9.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html # Important!
pip install -r requirements.txt
```

### Set up datasets

#### 0. Set up workspace
The workspace is the disk directory that stores datasets, training logs, checkpoints and results. Please ensure it has enough space. 
```
export workspace=$PATH_TO_YOUR_WORKSPACE
```
   
#### 1. Pre-trained model

Download the pretrained model from [dtu_pretrain](https://zjueducn-my.sharepoint.com/:f:/g/personal/haotongl_zju_edu_cn/EmR67vRXWZdJjvBtRlI4ENkB0RL6tSOUGxdP5NX4-QyXpA?e=8fckHh) (Pretrained on DTU dataset.)

Put it into $workspace/trained_model/enerf/dtu_pretrain/latest.pth

#### 2. DTU
Download the preprocessed [DTU training data](https://drive.google.com/file/d/1eDjh-_bxKKnEuz5h-HXS7EDJn59clx6V/view)
and [Depth_raw](https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/cascade-stereo/CasMVSNet/dtu_data/dtu_train_hr/Depths_raw.zip) from original [MVSNet repo](https://github.com/YoYo000/MVSNet)
and unzip. [MVSNeRF](https://github.com/apchenstu/mvsnerf) provide a [DTU example](https://1drv.ms/u/s!AjyDwSVHuwr8zhAAXh7x5We9czKj?e=oStQ48), please follow with the example's folder structure.

```
mv dtu_example.zip $workspace
cd $workspace
unzip dtu_example.zip
```
This script only shows the example directory structure. You should download all the scenes in the DTU dataset and organize the data according to the example directory structure. Otherwise you can only do evaluation and fine-tuning on the example data.

<!-- #### 2. NeRF Synthetic  -->
<!-- #### 3. Real Forward-facing -->
<!-- #### 4. ZJU-MoCap -->
<!-- #### 5. DynamicCap -->
<!-- #### 6. Custom Data -->

## Training and fine-tuning
Use the following command to train a generalizable model on DTU.
```
python train_net.py --cfg_file configs/enerf/dtu_pretrain.yaml 
```

## Evaluation

### Evaluate the pretrained model on DTU

Use the following command to evaluate the pretrained model on DTU.
```
python run.py --type evaluate --cfg_file configs/enerf/dtu_pretrain.yaml enerf.cas_config.render_if False,True gpus 1, enerf.cas_config.volume_planes 48,8 enerf.eval_depth True
```


```
{'psnr': 27.60513418439332, 'ssim': 0.9570619, 'lpips': 0.08897018397692591}
{'abs': 4.2624497, 'acc_2': 0.8003020328362158, 'acc_10': 0.9279663826227568}
{'mvs_abs': 4.4139433, 'mvs_acc_2': 0.7711405202036934, 'mvs_acc_10': 0.9262374398033109}
FPS:  21.778975517304048
```

21.8 FPS@512x640 is tested on a desktop with an Intel i9-12900K CPU and an RTX 3090 GPU.

## Visualization

## Interactive Rendering

## Citation

If you find this code useful for your research, please use the following BibTeX entry.

```
@inproceedings{lin2022enerf,
  title={Efficient Neural Radiance Fields for Interactive Free-viewpoint Video},
  author={Lin, Haotong and Peng, Sida and Xu, Zhen and Yan, Yunzhi and Shuai, Qing and Bao, Hujun and Zhou, Xiaowei},
  booktitle={SIGGRAPH Asia Conference Proceedings},
  year={2022}
}
```
