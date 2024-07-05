# YOLOv8 and Attention YOLOv8 (SE + ResNet and CMAM + ResNet)


## Architecture
<p align="center">
  <img src="img/figure_architecture.jpg" width="1024" title="details">
</p>

## Performance
| Model | Test Size | Param. | FLOPs | AP<sub>50</sub><sup>same</sup> | AP<sub>50-95</sub><sup>same</sup> | Speed | AP<sub>50</sub><sup>other</sup> | AP<sub>50-95</sub><sup>other</sup> |
| :--: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| YOLOv8+Ai1 | - | -.61M | -.9G |  -.58% | -.40% | -7.7ms | 0.62 | ---|
| YOLOv8+1vA | - | -.64M | -.4G |  -.25% | -.64% | -8.0ms | 0.62 | ---|
| YOLOv8+SE+Ai1 | - | -.64M | -.5G |  --24% | -.94% | -.7ms | 0.62 | ---|
| YOLOv8+SE+1vA | - | -.29M | -.5G | -.26% | -.00% | -.7ms | 0.62 | ---|
| YOLOv8+ResSE+Ai1 | - | -.64M | -.5G | -.24% | -.94% | -.7ms | 0.62 | ---|
| YOLOv8+ResSE+1vA | - | -.29M | -.5G | -.26% | -.00% | -.7ms | 0.62 | ---|
| YOLOv8+ResCBAM+Ai1 | - | -.29M | -.5G | -.98% | -.75% | -.1ms | 0.62 | ---|
| YOLOv8+ResCBAM+1vA | - | -.87M | -.2G | -.78% | -.16% | -.7ms | 0.62 | ---|


## Environment
```
  pip install -r requirements.txt
```

## Dataset
### Download the dataset
* You can download the Expanded Dataset on this [Link]([https://figshare.com/articles/dataset/GRAZPEDWRI-DX/14825193](https://www.kaggle.com/datasets/mohitsharmab21ee037/extended-dataset)).
  
### Split the dataset
* To split the dataset into training set, validation set, and test set, you should first put the image and annotatation into `./GRAZPEDWRI-DX/data/images`, and `./GRAZPEDWRI-DX/data/labels`.
* And then you can split the dataset as the following step:
  ```
    python split.py
  ```
### Directory Structure
* The dataset must contain training and validation (optional-testing).


       Trainable Dataset
          └── data   
               ├── meta.yaml
               ├── images
               │    ├── train
               │    │    ├── train_img1.png
               │    │    └── ...
               │    ├── valid
               │    │    ├── valid_img1.png
               │    │    └── ...
               │    └── test
               │         ├── test_img1.png
               │         └── ...
               └── labels
                    ├── train
                    │    ├── train_annotation1.txt
                    │    └── ...
                    ├── valid
                    │    ├── valid_annotation1.txt
                    │    └── ...
                    └── test
                         ├── test_annotation1.txt
                         └── ...


                      
### Data Augmentation
* working
  
## Methodology
* We have modified the model architecture of YOLOv8 by adding four types of attention modules, including <b>Shuffle Attention (SA), Efficient Channel Attention (ECA), Global Attention Mechanism (GAM), and ResBlock Convolutional Block Attention Module (ResCBAM)</b>.
<p align="center">
  <img src="img/figure_details.jpg" width="1024" title="details">
</p>

## Train & Validate
* We have provided a training set, test set and validation set containing a single image that you can run directly by following the steps in the example below.
* Before training the model, make sure the path to the data in the `./GRAZPEDWRI-DX/data/meta.yaml` file is correct.
```
  # patch: /path/to/GRAZPEDWRI-DX/data
  path: 'E:/GRAZPEDWRI-DX/data'
  train: 'images/train_aug'
  val: 'images/valid'
  test: 'images/test'
```

* Arguments

You can set the value in the `./ultralytics/cfg/default.yaml`.

| Key | Value | Description |
| :---: | :---: | :---: |
| model | None | path to model file, i.e. yolov8m.yaml, yolov8m_ECA.yaml |
| data | None | path to data file, i.e. coco128.yaml, meta.yaml |
| epochs | 100 | number of epochs to train for, i.e. 100, 150 |
| patience | 50 | epochs to wait for no observable improvement for early stopping of training |
| batch | 16 | number of images per batch (-1 for AutoBatch), i.e. 16, 32, 64 |
| imgsz | 640 | size of input images as integer, i.e. 640, 1024 |
| save | True | save train checkpoints and predict results |
| device | 0 | device to run on, i.e. cuda device=0 or device=0,1,2,3 or device=cpu |
| workers | 8 | number of worker threads for data loading (per RANK if DDP) |
| pretrained | True | (bool or str) whether to use a pretrained model (bool) or a model to load weights from (str) |
| optimizer | 'auto' | optimizer to use, choices=SGD, Adam, Adamax, AdamW, NAdam, RAdam, RMSProp, auto |
| resume | False | resume training from last checkpoint |
| lr0 | 0.01 | initial learning rate (i.e. SGD=1E-2, Adam=1E-3) |
| momentum | 0.937 | 	SGD momentum/Adam beta1 |
| weight_decay | 0.0005 | optimizer weight decay 5e-4 |
| val | True | validate/test during training |

* Example Train & Val Steps (yolov8m):
```
  python start_train.py --model ./ultralytics/cfg/models/v8/yolov8m.yaml --data_dir ./GRAZPEDWRI-DX/data/meta.yaml
```
* Example Train & Val Steps (yolov8m_ECA):
```
  python start_train.py --model ./ultralytics/cfg/models/v8/yolov8m_ECA.yaml --data_dir ./GRAZPEDWRI-DX/data/meta.yaml
```

## Related Works

<details><summary> <b>Expand</b> </summary>

* [https://github.com/RuiyangJu/Bone_Fracture_Detection_YOLOv8](https://github.com/RuiyangJu/Bone_Fracture_Detection_YOLOv8)
* [https://github.com/RuiyangJu/YOLOv9-Fracture-Detection](https://github.com/RuiyangJu/YOLOv9-Fracture-Detection)
* [https://github.com/RuiyangJu/YOLOv8_Global_Context_Fracture_Detection](https://github.com/RuiyangJu/YOLOv8_Global_Context_Fracture_Detection)

</details>
