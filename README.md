# YoloV3
________
YoloV3 Simplified for training on Colab with custom dataset. 

_A Collage of Training images_
![image](https://github.com/genigarus/YoloV3/blob/master/output/train.png)


We have added a very 'smal' Coco sample imageset in the folder called smalcoco. This is to make sure you can run it without issues on Colab.

Full credit goes to [this](https://github.com/ultralytics/yolov3), and if you are looking for much more detailed explainiation and features, please refer to the original [source](https://github.com/ultralytics/yolov3).

The credits for this goes to [TheSchoolOfAI](https://github.com/theschoolofai/YoloV3)

## Checking whether YoloV3 trains properly in colab. 
1. Create a folder called weights in the root (YoloV3) folder
2. Download from: https://drive.google.com/open?id=1LezFG5g3BCW6iYaV89B2i64cqEUZD7e0
3. Place 'yolov3-spp-ultralytics.pt' file in the weights folder:
  > * to save time, move the file from the above link to your GDrive
  > * then drag and drop from your GDrive opened in Colab to weights folder
4. Open python 3 notebook in google colab.
5. Clone this repository using the below command:
> `!git clone https://github.com/genigarus/YoloV3.git`
6. Run this command
> `python train.py --data data/smalcoco/smalcoco.data --batch 10 --cache --epochs 25 --nosave`

If training occurs properly, the next stop is training with custom dataset.

## For custom dataset:

### Dataset Creation

#### Download images

I downloaded 500 images of **Alex of Madagascar** which is not present in COCO dataset.

**Note:**
1. Ensure proper names of images.
2. Please ensure that the images are within 800x800 so that it fits properly in the annotation tool window.

#### Annotate images
1. Clone this repo: https://github.com/miki998/YoloV3_Annotation_Tool
2. Follow the installation steps as mentioned in the repo. 
3. Annotate the images using the Annotation tool.
4. This will create a separate label file for each image with image's file name within the *Labels* folder of YoloV3 Annotation tool.

#### Create dataset with structure as accepted by YoloV3

1. Place all the images in **images** folder and all the label files from above annotation tool in the **labels** folder.
2. Place both the **images** and **labels** folder within another folder. I have kept them in **customdata** folder.
3. Below is the folder structure which needs to be there:
```
data
  --customdata
    --images/
      --img001.jpg
      --img002.jpg
      --...
    --labels/
      --img001.txt
      --img002.txt
      --...
    custom.data #data file
    custom.names #your class names
    custom.txt #list of name of the images you want your network to be trained on. Currently we are using same file for test/train
```
4. As you can see above you need to create **custom.data** file. For 1 class example, your file will look like this:
```
  classes=1
  train=data/customdata/custom.txt
  test=data/customdata/custom.txt 
  names=data/customdata/custom.names
```
5. Below is the structure for **custom.txt** file. This is how the file looks like (please note the .s and /s):
```
./data/customdata/images/img001.jpg
./data/customdata/images/img002.jpg
./data/customdata/images/img003.jpg
...
```
6. You need to add **custom.names** file as you can see above. Since I downloaded images of Alex. My custom.names file looks like this:
```
alex
```
7. Alex above will have a class index of 0. 


### Architecture change
1. For COCO's 80 classes, YOLOv3's output vector has 255 dimensions ((4+1+80) * 3). Now we have 1 class, so we would need to change it's architecture.
2. Copy the contents of 'yolov3-spp.cfg' file to a new file called 'yolov3-custom.cfg' file in the data/cfg folder. In this file, make below changes:
 >i) Search for 'filters=255' (you should get three entries). Change 255 to 18 = (4+1+1) * 3
 >ii) Search for 'classes=80' and change all three entries to 'classes=1'.
 >iii) For few samples, it is a good idea to change:
  >>* burn_in to 100
  >>* max_batches to 5000
  >>* steps to 4000,4500

### Training and detection Yolo v3 on your custom dataset
1. Follow [this]() to train and detect your class.
2. Don't forget to perform the weight file steps mentioned in the section above. 
3. Run this command `python train.py --data data/customdata/custom.data --batch 10 --cache --cfg cfg/yolov3-custom.cfg --epochs 3 --nosave`

As you can see in the collage image above, a lot is going on, and if you are creating a set of say 500 images, you'd get a bonanza of images via default augmentations being performed. 


### Results
After training for 300 Epochs, results look awesome!

![image](https://github.com/genigarus/YoloV3/blob/master/output/download.jpeg)

<iframe width="560" height="315" src="https://www.youtube.com/embed/e1D-shteTAI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
