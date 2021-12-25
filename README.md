# LogoDetection
Logo Detection using Tensorflow (2) Object Detection API as part of the Final Project for Bocconi's Deep Learning Course. 

All the steps presented here can be found in the collab_tutorial section.

# Intro
In social media analysis, sometimes images/videos can inform us more than the pure text. For example, if you want to do a search on users of a certain brand, it is perhaps more effective to look for images/videos, as it is unlikely that anyone will explicitly write the company in their post. Therefore, combined with textual analyses, this methodology can improve the result of such analyses.

# Results
| mAP 0.5  | mAP 0.5:0.95 |
| ------------- | ------------- |
| 0.963  | 0.761  | 

# Workflow

## Object Detection API Install
Instaling Objet Detection API using Phyton in a Linux Machine. More information can be found [here](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2.md).

```
# Clone git repository
git clone https://github.com/tensorflow/models.git
cd models/research
# Compile protos.
protoc object_detection/protos/*.proto --python_out=.
# Install TensorFlow Object Detection API.
cp object_detection/packages/tf2/setup.py .
python -m pip install --use-feature=2020-resolver .
```

## Dataset
The dataset used for training was scrapped from public publications on instagran between 2015 and 2017 in some US cities. The logos used for training are: Nike, Adidas, Under Armour, Puma, The North Face, Starbucks, Apple Inc., Mercedes-Benz, NFL, Coca-Cola, Chanel, Toyota, Pepsi, Hard Rock Cafè.

## Generate TFRecords
TFRecords were created using the [official documentation](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/using_your_own_dataset.md)

## Pre-trained COCO models
Pre-trained coco models can be found [here](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md). For this project, it was used the CenterNet HourGlass104 512x512.
```
$ wget http://download.tensorflow.org/models/object_detection/tf2/20200713/centernet_hg104_512x512_coco17_tpu-8.tar.gz
```


## Recoment Data Structure
```
.
- data/
│   ├── eval-00000-of-00001.tfrecord
│   ├── label_map.txt
│   ├── train-00000-of-00002.tfrecord
│   └── train-00001-of-00002.tfrecord
└── models/
    └── my_model_dir/
        ├── eval/                 # Created by evaluation job.
        ├── my_model.config
        └── model_ckpt-100-data@1 #
        └── model_ckpt-100-index  # Created by training job.
        └── checkpoint            #
````


## Training
The official documentation for training and evaluation with Object Detection API, can be found [here](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_training_and_evaluation.md)

```
$ pipeline_file = PATH/TO/PIPELINE_FILE
$ model_dir = PATH/TO/MODEL_DIR
$ steps = 50000
$ train_file = PATH/TO/model_main_tf2.py
$ python ${train_file} \
    --pipeline_config_path={pipeline_file} \
    --model_dir={model_dir} \
    --alsologtostderr \
    --num_train_steps={steps} \
    --sample_1_of_n_eval_examples=1 
```

## Export Graph
```
$ output_directory = PATH/TO/EXPORT_GRAPH
$ python ${train_file} \
    --trained_checkpoint_dir {model_dir} \
    --output_directory {output_directory} \
    --pipeline_config_path {pipeline_file}
```

## Evaluation
```
$ python ${train_file} \
    --pipeline_config_path=${pipeline_file} \
    --model_dir=${model_dir} \
    --checkpoint_dir=${last_model_path} \
    --alsologtostderr
```

## IOU Results
| LOGO | IOU Header |
| ------------- | ------------- |
| THE NORTH FACE  | 0.920  |
| STARBUCKS  | 0.919  |
| TOYOTA  | 0.908  |
| MERCEDES-BENZ  | 0.905  |
| UNDER ARMOUR  | 0.899 |
| ADIDAS  | 0.898  |
| APPLE INC.  | 0.894 |
| HARD ROCK CAFE  | 0.894  |
| PUMA  | 0.873  |
| NFL | 0.868  |
| NIKE  | 0.868  |
| CHANEL  | 0.861  |
| PEPSI  | 0.838  |
| COCA-COLA  | 0.838  |

# License
MIT
