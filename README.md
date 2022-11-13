# Waterloo.AI Data Challenge <> Musashi AI
This computer vision project was completed by Steven Gong, Krish Parmar and Cheng Tang on November 12, 2022 for the Musashi AI Challenge. We placed 5th out of 25 teams.

The dataset was provided by Musashi AI, which is a ~15 minute feed (13501 images) of a static camera recording manufactoring factory from a Bird-Eye View (BEV). We are given 6 regions of interests (ROIs), highlighted in the image below:


No labels are given (just like how this would be in real life), and the challenge is to come up with an algorithm that can count the total number of frames for which a particular ROI is occupied in the entire video.


### Our Solution
##### Step 1. Setup
For our setup, we used two docker containers, one called (`musashi`) to conduct data exploration and run jupyter notebooks in, and one to train a YOLOv5-based model (called `yolov5`).

To get started, start up the containers
```bash
./spinup.sh yolov5

# In a new terminal
./build.sh musashi # build the image
./spinup.sh musashi # spinup a container, which we can run jupyter notebook in
```

You can then go into the terminal of the container by running:
```bash
docker exec -it profiles-musashi-1 /bin/bash # name of container might be different
python -m notebook --ip 0.0.0.0 --port 8888 --allow-root # start up jupyter notebook
```

This should start up a notebook that has been port forwarded to your local computer, which can be accessed at http://localhost:8887/


##### Step 2. Transfer Learning with YOLOv5
We hand labelled bounding boxes for 100 random images, which we used as our dataset. This was done using [Roboflow], the final dataset can be found [here](https://universe.roboflow.com/musashiai/musashiai). It is also on the current repository under `hand_labelled_yolo/`.

Here are the exact commands we followed to start training
```bash
docker exec -it profiles-yolov5-1  /bin/bash 

python train.py --data /musashi/yolo_config.yaml --weights yolov5x.pt --epochs 100 --batch 16 --freeze 10
```
The training command above freezes the backbone of the pretrained model, uses a batch size of 16, and trains it over 100 epochs.

##### Step 3. Inferencing and Generating an Answer
See the [final_answer_generation.ipynb](final_answer_generation.ipynb) notebook for the final answers we calculated.

The [inference.ipynb](inference.ipynb) notebook was used to generate a video of the output.

These final counts we submitted for each ROI for the entire video was a combination of the values below:
- `[ROI1, ROI2, ROI3, ROI4, ROI5, ROI6]`
- `[4886, 3189, 1512, 8531, 525, 2450]` (without inference thresholding)
- `[4886, 3180, 1507, 8384, 520, 2392]` (with inference thresholding at 0.3 confidence)


### Other Notes
After the event ended, they told us we had a score of ~78%. One explanation is that our IoU threshold is set to 0.01, which is too low. From their definition of occupancy, a ROI with simply a feet in it does not count as being occupied.

The winning team seemed to be using separate classifiers for each ROI, hence they had a multi-stage detection pipeline.

There were some other ideas we explored during this competition, but we didn't have enough time / implementation was a headache.

- Running Classical Image Classifcation
- Running [RAPID](https://github.com/duanzhiihao/RAPiD) for oriented bounding boxes
- Running YOLOv5 with support for rotation oriented bounding boxes. See resource [here](https://blog.roboflow.com/yolov5-for-oriented-object-detection/)


### Conclusion
Overall, this was a very great competition, we all learned a lot and look forward to participating in more data science competitions! When we will have extra time, we will also consider updating this repo and add additional features such as object tracking using [Norfair](https://github.com/tryolabs/norfair).

Shoot us a message on LinkedIn:
- [Steven Gong](https://www.linkedin.com/in/gong-steven/)
- [Cheng Tang](https://www.linkedin.com/in/cheng-tang-a584a71b2/)
- [Krish Parmar](https://www.linkedin.com/in/parmarkrish/)