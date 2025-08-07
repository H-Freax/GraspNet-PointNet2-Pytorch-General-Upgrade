# GraspNet Baseline: Upgraded for Modern PyTorch
This repository contains the official baseline model for the GraspNet-1Billion benchmark, now updated to support modern deep learning environments. Key upgrades include compatibility with Python 3.12, the latest PyTorch nightly builds (for CUDA 12.8+), and fixes for common compilation issues on newer NVIDIA GPUs (e.g., 40 and 50 series).

This version preserves the original model's functionality while ensuring it runs smoothly on up-to-date systems.


[[paper](https://openaccess.thecvf.com/content_CVPR_2020/papers/Fang_GraspNet-1Billion_A_Large-Scale_Benchmark_for_General_Object_Grasping_CVPR_2020_paper.pdf)]
[[dataset](https://graspnet.net/)]
[[API](https://github.com/graspnet/graspnetAPI)]
[[doc](https://graspnetapi.readthedocs.io/en/latest/index.html)]

<div align="center">    
    <img src="https://github.com/chenxi-wang/materials/blob/master/graspnet-baseline/doc/gifs/scene_0114.gif", width="240", alt="scene_0114" />
    <img src="https://github.com/chenxi-wang/materials/blob/master/graspnet-baseline/doc/gifs/scene_0116.gif", width="240", alt="scene_0116" />
    <img src="https://github.com/chenxi-wang/materials/blob/master/graspnet-baseline/doc/gifs/scene_0117.gif", width="240", alt="scene_0117" />
    <br> Top 50 grasps detected by our baseline model.
</div>

# Environment Requirements
This code has been tested and verified on the following environment:

OS: Ubuntu 24.04

GPU: NVIDIA 5090 with CUDA 12.8

Python: 3.12

PyTorch: Nightly Build (2.4.0.dev+)

Other Dependencies: open3d>=0.8, tensorboard, numpy, scipy, pillow, tqdm

## Installation
Get the code.
```bash
git clone https://github.com/H-Freax/GraspNet-PointNet2-Pytorch-General-Upgrade.git
cd GraspNet-PointNet2-Pytorch-General-Upgrade
```

### Install Python Dependencies
First, install PyTorch. For modern GPUs (like the NVIDIA 5090) and CUDA 12.8, you'll need the nightly build.



### Example for CUDA 12.8 - check the PyTorch website for the latest command
```bash
pip3 install --pre torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu128
```

Install packages via Pip.
```bash
pip install -r requirements.txt
```
Compile and install pointnet2 operators (code adapted from [votenet](https://github.com/facebookresearch/votenet)).
```bash
cd pointnet2
python setup.py install
```
Compile and install knn operator (code adapted from [pytorch_knn_cuda](https://github.com/chrischoy/pytorch_knn_cuda)).
```bash
cd knn
python setup.py install
```
Install graspnetAPI for evaluation.
```bash
git clone https://github.com/graspnet/graspnetAPI.git
cd graspnetAPI
pip install .
```

## Tolerance Label Generation
Tolerance labels are not included in the original dataset, and need additional generation. Make sure you have downloaded the orginal dataset from [GraspNet](https://graspnet.net/). The generation code is in [dataset/generate_tolerance_label.py](dataset/generate_tolerance_label.py). You can simply generate tolerance label by running the script: (`--dataset_root` and `--num_workers` should be specified according to your settings)
```bash
cd dataset
sh command_generate_tolerance_label.sh
```

Or you can download the tolerance labels from [Google Drive](https://drive.google.com/file/d/1DcjGGhZIJsxd61719N0iWA7L6vNEK0ci/view?usp=sharing)/[Baidu Pan](https://pan.baidu.com/s/1HN29P-csHavJF-R_wec6SQ) and run:
```bash
mv tolerance.tar dataset/
cd dataset
tar -xvf tolerance.tar
```

## Training and Testing
Training examples are shown in [command_train.sh](command_train.sh). `--dataset_root`, `--camera` and `--log_dir` should be specified according to your settings. You can use TensorBoard to visualize training process.

Testing examples are shown in [command_test.sh](command_test.sh), which contains inference and result evaluation. `--dataset_root`, `--camera`, `--checkpoint_path` and `--dump_dir` should be specified according to your settings. Set `--collision_thresh` to -1 for fast inference.

The pretrained weights can be downloaded from:

- `checkpoint-rs.tar`
[[Google Drive](https://drive.google.com/file/d/1hd0G8LN6tRpi4742XOTEisbTXNZ-1jmk/view?usp=sharing)]
[[Baidu Pan](https://pan.baidu.com/s/1Eme60l39tTZrilF0I86R5A)]
- `checkpoint-kn.tar`
[[Google Drive](https://drive.google.com/file/d/1vK-d0yxwyJwXHYWOtH1bDMoe--uZ2oLX/view?usp=sharing)]
[[Baidu Pan](https://pan.baidu.com/s/1QpYzzyID-aG5CgHjPFNB9g)]

`checkpoint-rs.tar` and `checkpoint-kn.tar` are trained using RealSense data and Kinect data respectively.

## Demo
A demo program is provided for grasp detection and visualization using RGB-D images. You can refer to [command_demo.sh](command_demo.sh) to run the program. `--checkpoint_path` should be specified according to your settings (make sure you have downloaded the pretrained weights, we recommend the realsense model since it might transfer better). The output should be similar to the following example:

<div align="center">    
    <img src="doc/example_data/demo_result.png", width="480", alt="demo_result" />
</div>

__Try your own data__ by modifying `get_and_process_data()` in [demo.py](demo.py). Refer to [doc/example_data/](doc/example_data/) for data preparation. RGB-D images and camera intrinsics are required for inference. `factor_depth` stands for the scale for depth value to be transformed into meters. You can also add a workspace mask for denser output.

## Citation
Please cite our paper in your publications if it helps your research:

```
@misc{qian2024thinkgrasp,
        title={ThinkGrasp: A Vision-Language System for Strategic Part Grasping in Clutter},
        author={Yaoyao Qian and Xupeng Zhu and Ondrej Biza and Shuo Jiang and Linfeng Zhao and Haojie Huang and Yu Qi and Robert Platt},
        year={2024},
        eprint={2407.11298},
        archivePrefix={arXiv},
        primaryClass={cs.RO}
    }
```


## References
This work builds on and refers to the following projects:

- [Vision-Language-Grasping](https://github.com/xukechun/Vision-Language-Grasping)
- [GraspNet-baseline](https://github.com/graspnet/graspnet-baseline)
- [Pointnet2_PyTorch](https://github.com/erikwijmans/Pointnet2_PyTorch)
- [ThinkGrasp](https://github.com/H-Freax/ThinkGrasp)

### Additional References
```
@article{pytorchpointnet++,
  Author = {Erik Wijmans},
  Title = {Pointnet++ Pytorch},
  Journal = {https://github.com/erikwijmans/Pointnet2_PyTorch},
  Year = {2018}
}

@inproceedings{qi2017pointnet++,
  title={Pointnet++: Deep hierarchical feature learning on point sets in a metric space},
  author={Qi, Charles Ruizhongtai and Yi, Li and Su, Hao and Guibas, Leonidas J},
  booktitle={Advances in Neural Information Processing Systems},
  pages={5099--5108},
  year={2017}
}

@article{fang2023robust,
  title={Robust grasping across diverse sensor qualities: The GraspNet-1Billion dataset},
  author={Fang, Hao-Shu and Gou, Minghao and Wang, Chenxi and Lu, Cewu},
  journal={The International Journal of Robotics Research},
  year={2023},
  publisher={SAGE Publications Sage UK: London, England}
}

@inproceedings{fang2020graspnet,
  title={GraspNet-1Billion: A Large-Scale Benchmark for General Object Grasping},
  author={Fang, Hao-Shu and Wang, Chenxi and Gou, Minghao and Lu, Cewu},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  pages={11444--11453},
  year={2020}
}

@INPROCEEDINGS{10161041,
  author={Xu, Kechun and Zhao, Shuqi and Zhou, Zhongxiang and Li, Zizhang and Pi, Huaijin and Zhu, Yifeng and Wang, Yue and Xiong, Rong},
  booktitle={2023 IEEE International Conference on Robotics and Automation (ICRA)}, 
  title={A Joint Modeling of Vision-Language-Action for Target-oriented Grasping in Clutter}, 
  year={2023},
  pages={11597-11604},
  doi={10.1109/ICRA48891.2023.10161041}
}
```

## License
This project is licensed under the MIT License. See the `LICENSE` file for more details.
