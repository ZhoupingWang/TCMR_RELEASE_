# Live Stream Temporally Embedded 3D Human Body Pose and Shape Estimation

Contact: 

[Zhouping Wang](wang.zhoup@northeastern.edu)

[Sarah Ostadabbas](ostadabbas@ece.neu.edu)

## Introduction
This repository is the official PyTorch implementation of [Live Stream Temporally Embedded 3D Human Body Pose and Shape Estimation]. 
The base codes are largely borrowed from [TCMR](https://github.com/hongsukchoi/TCMR_RELEASE).

## Installation
TePose is developed using python 3.7 on Ubuntu 18.04.
You may need sudo privilege for the installation.
```bash
source scripts/install_pip.sh
```

## Demo
- Download the pretrained SMPL layers from [here](https://smpl.is.tue.mpg.de) (male&female), [here](http://smplify.is.tue.mpg.de) (neutral) and SMPL regressor. Put SMPL layers (pkl files) under `${ROOT}/data/base_data/`. These opeartions can be realized with follwing code
```bash
source scripts/get_base_data.sh
```
- Download pre-trained [TePose weights](https://drive.google.com/drive/folders/14FcyVy49ryBH1AuVKgBXDNqjZsi-tivY?usp=sharing). 

The data directory structure should follow the below hierarchy.
```
${ROOT}  
|-- data  
|   |-- base_data  
|   |-- pretrained_models
|-- demo.py
:
:
|-- merged_courtyard_basketball_01.mp4
```
- Run demo with options (e.g. render on plain background). See more option details in bottom lines of `demo.py`.
- A video overlayed with rendered meshes will be saved in `${ROOT}/output/demo_output/`. 
```bash
python demo.py --vid_file merged_courtyard_basketball_01.mp4 --gpu 0 
```


## Running TePose

Pre-processed PoseTrack, 3DPW, MPI-INF-3DHP and Human3.6M are uploaded by TCMR authors from [here](https://drive.google.com/drive/folders/1h0FxBGLqsxNvUL0J43WkTxp7WgYIBLy-?usp=sharing).
Pre-processed InstaVariety is uploaded by VIBE authors [here](https://owncloud.tuebingen.mpg.de/index.php/s/MKLnHtPjwn24y9C).
AMASS can be generated following the instruction provided by VIBE authors [here](https://github.com/mkocabas/VIBE/blob/master/doc/train.md).
Download pseudo SMPL labels from [here](https://drive.google.com/drive/folders/1iLTMYMVo_BwRu3P-LpM_Bp1O6_e-xPh6?usp=sharing). You could generate it by yourself with code
```bash
source scripts/prepare_pseudo_thetas.sh
```

The data directory structure should follow the below hierarchy.
```
${ROOT}  
|-- data  
|   |-- base_data  
|   |-- preprocessed_data  
|   |-- pretrained_models
```

## Results
Here I report the performance of TCMR.


![table](./asset/table4.png)
![table](./asset/table6.png)

See [our paper](https://arxiv.org/abs/2011.08627) for more details.


### Evaluation

- Download pre-trained TCMR weights from [here](https://drive.google.com/drive/folders/1NxzmKw5QTGtOKgSQetkq66ZO-dbBSzrd?usp=sharing).  
- Run the evaluation code with a corresponding config file to reproduce the performance in the tables of [our paper](https://arxiv.org/abs/2011.08627).
```bash
# dataset: 3dpw, mpii3d, h36m 
python evaluate.py --dataset 3dpw --cfg ./configs/repr_table4_3dpw_model.yaml --gpu 0 
```
- You may test options such as average filtering and rendering. See the bottom lines of `${ROOT}/lib/core/config.py`.
- We checked rendering results of TCMR on 3DPW validation and test sets.

### Reproduction (Training)

- Run the training code with a corresponding config file to reproduce the performance in the tables of [our paper](https://arxiv.org/abs/2011.08627).
- There is a [hard coding](https://github.com/hongsukchoi/TCMR_RELEASE/blob/46462c664f1057fb3c14e2049a377e6bc071d622/lib/dataset/_dataset_3d.py#L92) related to the config file's name. Please use the exact config file to reproduce, instead of changing the content of the default config file.
```bash
# training outputs are saved in `experiments` directory
# mkdir experiments
python train.py --cfg ./configs/repr_table4_3dpw_model.yaml --gpu 0 
```
- After the training, the checkpoints are saved in `${ROOT}/experiments/{date_of_training}/`. Change the config file's `TRAIN.PRETRAINED` with the checkpoint path (either `checkpoint.pth.tar` or `model_best.pth.tar`) and follow the evaluation command.
- You may test the motion discriminator introduced in VIBE by uncommenting the codes that have `exclude motion discriminator` notations.


## Reference
```
@InProceedings{choi2020beyond,
  title={Beyond Static Features for Temporally Consistent 3D Human Pose and Shape from a Video},
  author={Choi, Hongsuk and Moon, Gyeongsik and Chang, Ju Yong and Lee, Kyoung Mu},
  booktitle = {Conference on Computer Vision and Pattern Recognition (CVPR)}
  year={2021}
}
```

## License
This project is licensed under the terms of the MIT license.

### Related Projects

[I2L-MeshNet_RELEASE](https://github.com/mks0601/I2L-MeshNet_RELEASE)  
[3DCrowdNet_RELEASE](https://github.com/hongsukchoi/3DCrowdNet_RELEASE)  
[TCMR_RELEASE](https://github.com/hongsukchoi/TCMR_RELEASE)  
[Hand4Whole_RELEASE](https://github.com/mks0601/Hand4Whole_RELEASE)  
[HandOccNet](https://github.com/namepllet/HandOccNet)  
[NeuralAnnot_RELEASE](https://github.com/mks0601/NeuralAnnot_RELEASE)
