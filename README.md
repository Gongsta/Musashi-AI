# Musashi-AI
Another dataset we downloaded: https://vip.bu.edu/projects/vsns/cossy/datasets/habbof/

Then, running YOLOv5 with support for rotation oriented bounding boxes.

https://blog.roboflow.com/yolov5-for-oriented-object-detection/

We use a version of YOLO that supports orientation

Training:
```bash
python train.py --data /musashi/yolo_config.yaml --weights yolov5s.pt --epochs 100 --batch 256 --freeze 10
```
python train.py --data /musashi/yolo_config.yaml --weights yolov5x.pt --epochs 100 --batch 16 --freeze 10


We freeze the backbone.


Pipeline
1. Preprocess the image to generate crops for each ROI
2. Create a small validation set to evaluate performance


Model:
- Most of the regions seem not to have any person, however there are potentially other people in the region.

Internal Evaluations:
- We hand-labeled about 100 images which we used as our validation set