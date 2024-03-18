# DmilPullDataPushContextFramework_MXSP5

Date: 01/19/2022

**MIL Version** MIL X SP5  

**Type** New example

**Description**  
This program pulls data to a server and pushes a new context to one or multiple clients using Distributed MIL.

The project structure, including the xml and png files, are intended to be copied in "\Users\Public\Documents\Matrox Imaging\MIL\Examples\Processing\Classification\DmilPullDataPushContextFramework" of the MIL installation directory and to be displayed by the MIL example launcher.

**Link**  
https://github.com/Zebra-Aurora-Imaging-Library/DmilPullDataPushContextFramework_MXSP5

**Story line**  
Imagine a factory with 10 production lines, each of which has a smart camera that performs image classification. To do this, a trained classifier context is also involved. For various reasons, such as adding a new class, or new setup conditions, we might need to update the context. At the moment, to update the context, we need to stop the application, replace the context and run the application/line, which can be time consuming.  In this example, we will show how to update the context transparently (that is, on the fly). This is done by connecting the cameras to a Client and using distributed MIL to upload the context into the cameras, preprocess them, and update the used context without any delay. 

Now imagine that we want to automate the fine-tuning process as well. That is, we want to automatically gather new images that we consider critical to the image classification solution, such as images with low prediction scores. This means all the cameras will be saving images, and after some days or weeks, we will have a new dataset of challenging images which we can use to improve our solution (classifier). To do this, the client must gather all the images from all the cameras. Note, although the saved images will be labeled by the system, an operator should verify the labels and fix the mislabeled ones. The more labels we fix, the better the improvement. 
After preparing the new dataset merged with the original dataset, we can fine-tune the context and upload it to each camera. 

**Note**  
1-This example is developed based on the MIL classification module, deep learning technology and edge-cloud architecture. However, it can be used for most context based modules. 
2-This example uses the 10 pasta images and the context from the MIL example, "Mclass".
3-In the mega-data images UUID_MD.mim that are saved with the original low-scored images UUID_IM.mim, we also save an array that contains the prediction index, score, and time stamp, which can be opened in MIL CoPilot.

**Server side (i.e. the system that grabs live images)**
There are three folders:
1- Contexts:  Contains all the contexts (original and the updated).
2- Inference: The original solution (trained classifier) running on the server.
3- SavedImages: The location to save all the critical original images that have a low prediction score, named "UUID_IM.mim". For each image, a UUID_MD.mim that contains mega data are saved: prediction index, score and time.

Tasks on the server side:
1- Save the images: This part could vary based on the original solution. 
Since the client is constantly looking for new files, and it could start downloading an image file that has not been completely saved on camera, we save the images first with a ".tmp" extension and then rename them to ".mim" when the file is completely saved. 
The saving process must be done using a low priority thread to prevent any slow down. 
Make sure the number of images that you save does not overflow the disk. 
2- Update the context: 
This process constantly looks for new uploaded contexts using a low priority thread to prevent any slow down. 
After detecting the context, it replaces the original context with the updated one on the hard drive. It then loads the updated context, preprocess it (with the ID of the used context), and frees the previous context. 

**Client side**
There are two applications:
1- Uploader:  uploads updated contexts to the server.
2- Collector: downloads the low-scored images and mega-data images from all the servers, then deletes all those images that were saved on the servers.

**Test systems**
Windows 10 64-bit and VS2017.
