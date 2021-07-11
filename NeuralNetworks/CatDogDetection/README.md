# Assignment: Recognize cats and dogs

In this assignment you are going to build an app which must be able to identify cats and dogs in any image. 

The easiest way to do this is to build a neural network and train it on a dataset of cat and dog images. The **Kaggle cats and dogs** dataset has exactly what we need.

You can [download the dataset](https://www.dropbox.com/s/dnhezyz3g6i8s88/catsanddogs2.zip?dl=1) from my DropBox account. Save the archive in a local folder but make sure you DO NOT unzip the file. You won't need to becasue Azure Machine Learning Studio can work with zipped training files directly.

Here's what the dataset looks like:

![MNIST digits](./assets/datafile.png)

These are 2000 pictures of cats. We also have a second set with 2000 images of dogs. The zipped file is slightly over 100 MB in size.

You are going to train a neural network on these image sets and get the cat and dog detection accuracy as high as possible.

Let’s get started. 

## Prepare the dataset

The first thing you need to do is upload the cats and dogs files into Azure Machine Learning as a new dataset. 

Open the Azure Machine Learning Studio and navigate to the datasets list. Create a new dataset and make sure to specify the following details:

* Name: cats-and-dogs
* Dataset type: File

![Create dataset step1](./assets/new/image2.png)

Click the blue Next button and then select the **catsanddogs2.zip** file you downloaded earlier. 

![Create dataset step2](./assets/new/image4.png)

Click the blue Next button and confirm your new dataset.

![Create dataset step2](./assets/new/image6.png)

You now have all 4000 images of cats and dogs uploaded in Azure Machine Learning and ready for training a deep neural network. 

## Building the training pipeline

Open the Azure Machine Learning Designer and create a new pipeline. Then set the compute cluster to the NC6 GPU cluster you created in the digit recognition assignment.

Recognizing cats and dogs is a very compute-intensive operation, and we'll need the extra power of the NC6 cluster to complete machine learning training in a timely manner.

![Build training pipeline step 1](./assets/new/image14.png)

Now open the datasets group and drag the cats-and-dogs dataset onto your designer canvas:

![Build training pipeline step 1](./assets/new/image16.png)

This is the file dataset containing our 4000 cat and dog images. But we can't train on this dataset yet. We first need to convert it to an **image directory**: a specialized datastructure for training machine learning models on image collections.

The cats and dogs dataset stores all cat images in a folder named **/cat/** and all dog images in a folder named **/dog/**. Azure Machine Learning has built-in support for this kind of directory structure and will automatically treat the folders as labels.

When we create an image directory, Azure will correctly label each image and then randomize the set so that the neural network alternately sees a random cat and then a random dog during training. This setup minimizes the risk of bias. 

Let's set this up. Open the Computer Vision group and drag the Convert To Image Directory module onto the pipeline canvas. Then connect the dataset to the Image Directory module:

![Build training pipeline step 2](./assets/new/image17.png)

You don't need to configure the Image Directory module. It will automatically scan the zip file, recognize the directory structure, and create a training set with all the images correctly labelled.

Our next step is to resize all images. Deep neural networks can only train on images that are all the same size. But our cats and dogs dataset has images with all kinds of different sizes and orientations. So we need to resize each image to a common baseline. 

Drag the Init Image Transformation module from the Computer Vision group onto the designer canvas. Configure it as follows:

* Resize: True
* Size: 256
* Center crop: True
* Crop size: 224
* Pad: False
* Color jitter: False
* Grayscale: False
* Random resized crop: False
* Random crop: False
* Random horizontal flip: False
* Random vertical flip: False
* Random rotation: False
* Random affine: False
* Random grayscale: False
* Random perspective: False

This will resize each image to 256x256 pixels and then crop the images using a 224x224 box from the center of the image.

![Build training pipeline step 3](./assets/new/image19.png)

Now we need to apply this image transformation to the image directory. 

Drag an Apply Image Transformation module from the Computer Vision group to the designer canvas. Connect its **leftmost** input to the Init Image Transformation module and its **rightmost** input to the Convert To Image Directory module. 

Configure the module as follows:

* Mode: For training

![Build training pipeline step 4](./assets/new/image23.png)

Now we're going to split the image directory. We'll reserve 80% of the images for training and 20% of the images for testing. 

We can't use the regular split module because it can't operate on image directories. But fortunately there's a specialized module in the computer vision group that can handle splitting image directories. 

Drag the Split Image Directory module from the Computer Vision group onto the designer canvas. Connect its input to the Apply Image Transformation module.

Then configure the split as follows:

* Fraction of images in the first output: 0.8

![Build training pipeline step 5](./assets/new/image25.png)

We're going to use a trick for this assignment. Instead of training a deep neural network from scratch, we're going to grab a **pretrained** neural network from the Internet and retrain it on our dog and cat pictures. This process is called **fine-tuning** and it's a very popular technique to very quickly build an object detector. 

We will use the ResNet neural network in this assignment. This machine learning model is available out of the box in Azure Machine Learning Studio. 

Drag the ResNet module from the Computer Vision group onto the designer canvas. Configure it as follows:

* Model name: resnetx101_32x8d
* Pretrained: checked

The important setting here is **pretrained**. This tells Azure ML to download the pretrained neural network from the Internet and use it for our cat and dog challenge. This will dramatically reduce our training time.

![Build training pipeline step 6](./assets/new/image27.png)

ResNet is a PyTorch model, so we need a specialized training module to train this Python model on the image directory. 

Open the Model Training group and drag the Train PyTorch Model module on the designer canvas. Connect its **leftmost** input to the ResNet module, its **middle** input to the **leftmost** output of the Split Image Directory module, and its **rightmost** input to the **rightmost** output of the Split Image Directory Module. 

Then configure the module as follows:

* Epochs: 5
* Batch size: 16
* Learning rate: 0.001
* Random seed: 1
* Patience: 3

![Build training pipeline step 7](./assets/new/image30.png)

We now have a fully-trained PyTorch neural network capable of detecting cats and dogs in images. All that remains is to score the network on the test data and evaluate the model metrics. 

Open the Model Scoring & Evaluation group and drag the Score Image Model module onto the designer canvas. Connect it to the Train PyTorch module. 

Then drag the Evaluate Model module onto the canvas and connect it to the Score Image Model module.

![Build training pipeline step 8](./assets/new/image32.png)

The evaluation module will treat this as a multiclass classification problem and calculate the accuracy and micro- and macro precision and recall. 

Your completed pipeline should now look like this:

![Completed pipeline](./assets/new/image33.png)

Run the pipeline in a new experiment, and check out the evaluation results after the run has completed.

## Your results

What results do you get? What is your overall accuracy and your micro- and macro precision and recall? 

Are you happy with these results?
