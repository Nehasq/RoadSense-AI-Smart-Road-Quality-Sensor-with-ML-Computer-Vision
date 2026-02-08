# RoadSense AI

RoadSense AI is a pothole detection system designed for UK roads. The primary focus of this work was the image-based component, which involved developing a system to detect potholes from camera images using machine learning and computer vision techniques, specifically employing object detection with YOLOv8 — a major part of my dissertation research. This task included designing a protocol for efficient data collection, labeling, and preprocessing, followed by implementing and evaluating a model to accurately identify potholes.

---

## 📁 Datasets

This repository contains links, descriptions, and documentation for three image datasets used for analysis, experimentation, or model development.

Due to GitHub file size limitations, the public dataset can be downloaded using the link below, and the other variations of the dataset were created by applying the data augmentation techniques mentioned below.

### Dataset 1: Roboflow Dataset

**Description:** contains 665 pothole images, with variations in illumination, background noise, shadows, water-filled potholes, and moving vehicles. A comprehensive analysis was conducted, all objects in the dataset 
are marked with bounding boxes and labelled as potholes, meaning that no negative training examples or images without potholes are included. The dataset was sourced from Roboflow, which, in turn, was originally sourced 
from Kaggle (Chitholian, 2020) under the “ODbL v1.0” license. The original Kaggle version didn’t have a validation set, so the Roboflow platform reshuffled the data and created a validation set, which was then downloaded 
in YOLO format.

The dataset was divided into training, validation, and test sets to prevent overfitting and accurately evaluate the model. The dataset was split in a 70:20:10 ratio, a commonly used ratio among researchers, with 70% for 
training, 20% for validation, and 10% for testing. This distribution resulted in a training dataset comprising 465 images, a validation set containing 133 images, and a testing dataset consisting of 67 images.

**Download link:** https://universe.roboflow.com/brad-dwyer/pothole-voxrl/dataset/1

### Dataset 2: Augmented Roboflow Dataset

**Description:** The original dataset consisted of 465 training images, 133 validation images, and 67 test images. Various augmentations were applied using the Roboflow platform. Special care was taken when uploading the 
dataset by selecting the option to "keep the original split" to maintain the integrity of the data distribution across training, validation, and test sets. The dataset was pre-processed by auto-orienting and resizing the 
images to 640x640, the default image size for YOLOv8 and YOLOv10. Augmentations such as rotation, flips, and brightness adjustments were applied. Preprocessing was performed on the entire dataset, whereas data augmentations 
were applied only to the training set. Following common practice, the validation and test sets remain unchanged. Each training sample generated three output variations, resulting in a total of 1,592 images. The dataset's final
distribution, including training(1392), validation(133), and test sets(67).


### Dataset 3: Custom UK Road Dataset

**Description:** The public dataset provided a starting point for comparing the results of our model with state-of-the-art (SOTA) models. However, our goal was to develop a pothole detection system specifically adapted to UK roads. 
Since there was little to no data available for UK roads, we needed to collect our own data to create a new dataset better suited to our needs.

- **Data Collection & Annotation Overview:** 

To collect road anomaly data, a car trip was conducted around and beyond the University area to capture both sensor and image data. The objective was to record events that cause fluctuations in sensor signals and enable differentiation 
between road anomalies such as potholes and speed bumps.

- **Data Collection Setup:**

Two cars were used: one equipped with a single sensor and the other with two sensors, all mounted at the front and oriented identically. Each car was also fitted with a camera (GoPro or dashcam) to record road footage. Sensor data was 
collected using the SensorTile.box Pro (STMicroelectronics), which records 3-axis accelerometer and gyroscope data and stores it in CSV format via the STBLE Sensor app.

During the drive, passengers manually logged road anomalies and “Start” events using a Python script that recorded keypresses with millisecond-level timestamps. The trip lasted approximately 1 hour and 50 minutes, covering areas dominated 
by speed bumps and potholes. Sensors recorded continuously throughout the trip, though part of the data could not be labelled due to missing event logs.

- **Recorded Road Events:**

The primary road events collected were potholes, speed bumps, and manholes. Other surface irregularities (e.g. cracks or rough road sections) were grouped under an “Other” label.

- **Data Synchronisation:**

To synchronise sensor and video data, sensors were dropped from a height onto a metal tray, creating a clear signal peak and audible cue in the video. Each drop was also logged as a “Start” event via the Python script. This process was 
repeated multiple times during the trip to ensure accurate alignment between sensor and camera timestamps.

- **Annotation Process:**

Sensor data was segmented into 2-second windows (100 records per segment), each labelled with a single anomaly. Duplicate entries were removed, leaving one record per segment. Video footage was then aligned with sensor data using relative 
timestamps, accounting for delays and variations in vehicle speed.

Frames were extracted from the video—one per segment—to ensure optimal visibility of road conditions. All frames were reviewed, with private information (e.g. faces and number plates) blurred or cropped. Annotations were performed using 
CVAT, labelling potholes explicitly and assigning “road” labels to frames without anomalies. All annotations were exported in YOLO format for model training.

The final dataset consists of 158 annotated frames and intervals, with sensor records linked to their corresponding image paths.

---


