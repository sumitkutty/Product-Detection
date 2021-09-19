##Files:
* Training.ipynb: Training script
* Object Detection.ipynb : Data Preparation and Detection Script
* Output Files: contains the deliverables.
* Tensorflow: Contains the Object Detection API
* requirements.txt: All Dependancies
* ShelfImages: The original given train and test set, along with the generated pascal-voc dataset.
* Arial.tff: The font file for bounding box label.


# Objectives:
* To detect and count the number of products on a shelf using Computer Vision.

## Framework/Algorithm used:
* Framework: Tensorflow's Object Detection API.
* Architecture: ssd mobile net V2

## Dataset:
* The dataset contains of 2 folders: train and test. The train folder consists of 283 images and the test contains another 71 images.
* Annotations for the trains set is given in one CSV file, where each row contains details of the bounding box for each product.


## Dataset Preparation:
* As the annotations were given in CSV format, and the TF API requires annotations in an PASCAL-VOC xml file, The annotations in the csv file were converted to PASCAL-VOC using the roboflow service online. No other pre-processing was applied.
* Each pascal voc file contained the information of all the objects in one image. The most important being the bounding box coordinates of all the objects in the image which the API will use to train.
* The 'name' tag in all the pascal voc files was changed to 'product' as there is only one object to detect.
* At this point, There are 283 images and 283 xml files.
* The dataset was split into train and test sets, where training consisted of 253 images/xml files, and the test set had the rest.
* A label map was created with one 'class' in order to map the class name to an integer, which Tensorflow uses for training and detection.


## Constructing TF records:
* The train and test set were converted and stored as tf records, which is a serialized binary format that tensorflow uses to allow convenient storage and import. 
* The records were constructed using the images and xml files using a pre-written script from tensorflow's API website.

## Training/Detection Network:
* The network selection was done from one of the model zoo's pre-trained model provided by the API.
* SSD MOBILE NET V2 was used for the detection. One of the main reason to choose mobilenet was to avoid excess overhead and decrease model complexity as GPU resources were limited.
* The training was done from the last checkpoint of the pre-trained model.
* The GPU on Google Colab was used to train the net, that took approximately 3 hours. 
* As there was only 1 object class to detect, the mobilenet did a good enough job.
* The training generated a total of 3 checkpoints uptill ckpt-0.

## Hyperparameters:
* The number of steps set was 2500
* Batch Size: 4 
-> All other default hyperparameters including learning rate and decay were kept the same.

## Detection Tuning:
* The confidence threshold was set to 0.5301, which was optimal to find only the products and not find redundant bounding boxes.


## Evaluation:
* coco detection metrics was used to evaluate the model on the test set.
* The mAP , AP and AR values were obtained, and were stored in the json file.

Note: The dataset preparation and custom product detection was done on the local machine, and only the training was done on COLAB, to leverage GPU benefits. The training and detection scripts can be found in two different notebooks.
