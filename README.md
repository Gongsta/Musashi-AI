# Musashi-AI
Another dataset we downloaded: https://vip.bu.edu/projects/vsns/cossy/datasets/habbof/

Then, running YOLOv5 with support for rotation oriented bounding boxes.

https://blog.roboflow.com/yolov5-for-oriented-object-detection/

We use a version of YOLO that supports orientation

YOLO_OBB


Pipeline
1. Preprocess the image to generate crops for each ROI
2. Create a small validation set to evaluate performance


Model:
- Most of the regions seem not to have any person, however there are potentially other people in the region.

Internal Evaluations:
- We hand-labeled about 100 images which we used as our validation set