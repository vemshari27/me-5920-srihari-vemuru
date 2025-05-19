
  

# Yield Forecasting via DeepSORT/StrongSORT Tracking

  

**Team Members: Srihari Vemuru, Rehan Rahimoon**

  

This is the code to run DeepSORT and StrongSORT on our lettuce dataset.

  

## Data

  

We downloaded lettuce data from [LettuceMOT](https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2022.1047356/full) and [LFSD Dataset](https://ieee-dataport.org/documents/lfsd-dataset). It can be downloaded by visiting their websites. We have uploaded our entire data in this box link.

  

### Detection

  

DeepSORT and StrongSORT are two stage object trackers. They need a YOLO object detector in the first step of their process. For this, we used *straight2* video from LettuceMOT to train a YOLOv8s model. The training results can be found in [eda.ipynb](eda.ipynb). The data for this is in `data/letttucemot`.

  

### Data Augmentation

  

Here we create the simulation real world issues in the dataset to check how the models perform. The code for this is in `Tracking/Data/LettuceMOT/Jitter and Occlusion` sections of eda.ipynb. The resulting data in the data folder. Here is an occlusion example.

  

![alt text](../../project/data/lettucemot/straight1_occ/img/000002.png)

  

In the code, you can notice that it requires changing the GT info also. We skipped info of four frames in the GT labels, when we add jitter. We change the bbox dimension of the GT labels when we add the black bar for occlusion.

  

## Tracking

We found [BoxMOT](https://github.com/mikel-brostrom/boxmot.git)'s plug and play code perfect for our project. We used the following commands to run the tracking.

  

> python tracking/track.py --yolo-model /home/shinobi-owl/ISU/sem2/ME_5920/project/runs/detect/train2/weights/yolov8.pt --tracking-method strongsort --source /home/shinobi-owl/ISU/sem2/ME_5920/project/data/lettucemot/straight1/img

  

This created a output labels files in `assignments/me-5920-srihari-vemuru/runs/mot`. It creates txt files with the results. We use BBox+ID Plotting section in eda.ipynb to plot bboxes over the images based on the output to visualize the outputs.

  

![alt text](../../project/data/results/strongsort/B&F1/000003.png)

  

We create a video out of these images using `ffmpeg -framerate 10 -i O\&I1/%06d.png -c:v libx264 -pix_fmt yuv420p oi.mp4`

## Evaluation
We evaluate DeepSORT and StrongSORT on metrics MOTA and HOTA. For this we have to run 
> python tracking/val.py --yolo-model /home/shinobi-owl/ISU/sem2/ME_5920/project/runs/detect/train2/weights/yolov8.pt --tracking-method strongsort --source /home/shinobi-owl/ISU/sem2/ME_5920/project/data/lettucemot/straight1/img