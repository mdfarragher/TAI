# The case

New York city hosts many taxi companies that ferry passengers from all over the city to their destinations. But are these companies charging fair prices?

You'll be happy to learn that the city has an oversight commission called the **NYC Taxi & Limousine Commission** that tracks all daily trips and payments throughout the city. The commission provides up-to-date public datafiles with trip data for each month of the year.  

So here's what we could do: we could build a machine learning model and train it on the NYC T&LC datafiles, and then use the model to predict taxi prices. 

If we get low RMSE and MAE metrics, it means the model is accurately predicting taxi prices accross the board and this proves that NYC taxi companies are generally fair. 

In this case study, you're going to help NYC consumers by training a machine learning model on the NYC T&LC data, evaluate your model, and report the RMSE and MAE results. 

Do you think taxi prices in New York are fair?

Let's find out!

## The dataset

The first thing you'll need is a data file with transcripts of New York taxi rides. The [NYC Taxi & Limousine Commission](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page) provides yearly TLC Trip Record Data files which have exactly what you need.

Let's work with the datafile for December 2018. Download the corresponding [Yellow Taxi Trip Records](https://s3.amazonaws.com/nyc-tlc/trip+data/yellow_tripdata_2018-12.csv) and save the file as **yellow_tripdata_2018-12.csv**. 

This is a CSV file with 8,173,233 records that looks like this:

￼
![Data File](./assets/data.png)


There are a lot of columns with interesting information in this data file. Check out:

* Column 0: The data provider vendor ID
* Column 1: The picup date and time
* Column 2: The dropoff date and time
* Column 3: The number of passengers
* Column 4: The total trip distance
* Column 5: The rate code (standard, JFK, Newark, …)
* Column 9: Payment type (credit card, cash, …)
* Column 10: The fare amount

You can select any combination of columns you like to train your model. 

## Getting started

Start by uploading the NYC T&LC datafile into Azure.

To save time, you can do this directly in your workspace. You don't have to set up a storage account and a datastore first. 

Just go to the Datasets page in your Azure Machine Learning workspace, click the +Create Dataset button, and then select From Local Files from the dropdown menu:

![Create new dataset](./assets/newdataset.png)

This will upload the NYC T&LC file directlty into your workspace, ready for use in a pipeline.

The pipeline for this case study is going to be very similar to the California Housing pipeline you created in the previous lesson. So to get started quickly, simply clone that pipeline: 

![Clone the pipeline](./assets/clonepipeline.png)

Then you only need to make the following changes:

* Replace the dataset module with the NYC T&LC dataset
* Alter the SQL statement in the SQL Transformation module to perform whatever data processing you need
* Select the correct label column in the Train Model module.

## Set up a GPU cluster

This assignment features a lot of training data which migh be too much for our basic D1 compute cluster. To speed up the pipeline we're going to have to quickly provision some heavy duty hardware.

Go to the Compute page in Azure ML Studio and select the Compute Clusters tab. 

Click the +New button, and add the following cluster:

* Compute name: [choose a name for the cluster]
* Virtual machine type: GPU
* Virtual machine priority: Low priority
* Virtual machine size: Standard_NC6
* Minimum number of nodes: 0
* Maximum number of nodes: 2
* Idle seconds...: 120

![Setup GPU cluster](./assets/new-cluster.png)

Click the blue Create button and wait until the cluster is up and running. 

Then when you create your training pipeline, click the gear icon at the top of the page, and in the information panel switch the pipeline over to the new compute cluster.

![Set compute cluster](./assets/set-cluster.png)

Your pipeline will now run on the new cluster. This can make your training time up to 100x faster. 

## Your assignment

I want you to build a pipeline that reads the data file, processes it, and then trains a linear regression model on the data.

You can select any combination of input features you like, and you can perform any kind of data processing you like on the columns.

Partition the data and use the trained model to make taxi fare predictions on all the trips in the test partition. Calculate the best possible RMSE and MAE and share it in our Slack homework group.

See if you can get the RMSE as low as possible. Share in our group how you did it. Which features did you select, how did you process them, and how did you configure your model?

Good luck!

## Cleaning up

The NC6 virtual machines are expensive to run, so make sure to delete the GPU cluster after you have completed this assignment!

If you keep the cluster running, you will very quickly spend all of your free Azure credits and your account will be disabled. 
