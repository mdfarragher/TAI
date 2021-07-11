# Assignment: Set up Azure Machine Learning Studio

In this assignment you will set up the Azure Machine Learning workspace that you will use throughout the rest of this training.

## Before You Start

To complete the assignments in this course, you will need an Azure subscription. If you do not already have one, you can sign up for a free trial at [https://azure.microsoft.com](https://azure.microsoft.com).  

## Create a new Azure Machine Learning Workspace

Let's get started. Your first task is to set up an Azure Machine Learning Workspace. This is a centralized place to manage all of the Azure machine learning resources you need to work on a project.

Go to your Azure portal page. It should look something like this:

![Azure Portal](./assets/portal.png)

This is a screenshot of my own Azure portal, yours will have a different list of Azure services and recent resources. 

Now click the Create A Resource button (with the blue plus sign icon) and search for "machine learning". Select the Machine Learning result from the dropdown list:

![Setup step 1](./assets/step1.png)

You'll see an overview page with information about Azure Machine Learning. 

Click the Create button to confirm your choice:

![Setup step 2](./assets/step2.png)

Now create a new machine learning workspace and provide the following details:

* The Azure subscription in which you will place the workspace. Select your Azure Pass Sponsorship from the list.
* The resource group in which you will place the workspace. It's a good idea to create a new resource group for this training and give it a descriptive name, for example: 'rg-techionista-dp100'. 
* The name of the machine learning workspace. Give your workspace a nice unique name that contains the words 'techionista', 'dp100' and perhaps your initials too.
* The region in which to place the workspace. This corresponds to the Azure datacenter that will host your machine learning files. You should choose the location that's closest to where you are working to minimize latency, and that would be 'West Europe' (which refers to Microsoft's datacenter in Middenmeer in the Netherlands).

The Storage account, Key vault and Application insights will be automatically filled in as you type the workspace name. You do not have to change these fields and can leave them at their default values.

You also don't need to change the Container registry field. We do not need a specific container registry in this training.

![Setup step 3](./assets/new/image1.png)

Now click the blue Review+Create button, and then click Create. Your machine learning workspace will now be deployed. This can take a couple of minutes.

After deployment is complete, you'll be notified like in the image below. Click the Go To Resource button to access your new workspace.

![Setup step 4](./assets/new/image2.png)

## Access the Azure Machine Learning Studio interface

You can now manage your workspace in the Azure portal, but it contains lots of information that's specifically intended for cloud administrators.

For us machine learning practicioners Microsoft has provided a much better web interface that's specially designed for managing machine learning resources. This interface is called the Azure Machine Learning Studio. 

You can access the studio interface by clicking the blue 'Launch studio' button:

![Setup step 5](./assets/new/image3.png)

This will launch the Azure Machine Learning Studio. 

You're now looking at the Azure Machine Learning Studio interface. We will be spending most of our time here while we work through the assignments in this course. 

![Azure Machine Learning Studio](./assets/new/image4.png)

## Create a Compute Instance

Azure Machine Learning Studio uses 'compute resources' to train and run machine learning models. These are clusters of virtual machines that work together to efficiently train and run very large ML models. You can create as many clusters as you like, and each cluster can have as many virtual machines as you like. Depending on the size of the training job, Azure Machine Learning will automatically boot up more VMs to the cluster to handle the load.

For this training you will need only a tiny cluster. We will limit the total number of virtual machines to 2 to keep costs down. 

To get started, you'll need to set up the following:

* A Compute Instance. This is a single virtual machine that hosts a Jupyter Labs installation that provides a cloud-based Python notebook environment for machine learning practicioners. We can also train small machine learning models with this single virtual machine.

* A Compute Cluster. This is a scalable cluster of multiple virtual machines that can be used to train large-scale machine learning models. 

Let's set up both of them right now. 

In the Azure Machine Learning Studio you'll notice a menu bar on the left. In this menu click on the Compute link (it's in the Manage section near the bottom). 

![Compute setup step 1](./assets/new/image5.png)

You're now on the compute page. You'll notice that there are no compute instances configured yet and the list is empty. 

Make sure the Compute Instance tab is selected, and then click on the blue +New button:

![Compute setup step 1b](./assets/new/image6.png)


To create a new compute instance, specify the following:

* The name of the new compute instance. Type a nice name like 'techionista-compute' or something else.
* The virtual machine type, this can be either CPU or GPU. Select CPU for this training to keep costs low. 
* The virtual machine size. Click 'Select from all options'  and then type 'DS11' in the search field. The search result list will then show the 'Standard_DS11_v2' size. Select it.

Note that we're selecting the smallest possible virtual machine size for this training in order to keep daily costs as low as possible. 

If you want, you can also select the GPU virtual machine type and enter 'NC6' in the search field. This will display the 'Standard_NC6' size. This virtual machine will give you better performance but at a higher cost.

Now click the blue Create button.

![Compute setup step 2](./assets/new/image8.png)

The compute instance will now be created, this can take a couple of minutes. When the instance is ready, you will see it appear in the compute instance list with the status 'Running':

![Compute setup step 2](./assets/new/image10.png)

## Create a Compute Cluster

Make sure you select the Compute Cluster tab at the top of the page, and then click the blue +New button. 

![Compute setup step 2](./assets/new/image12.png)

We're going to set up a new cluster by providing the following information:

* The virtual machine priority. Set this to Low priority to save money. 
* The virtual machine type. Just like with the compute instance, select the CPU type here.
* The virtual machine size. Click 'Select from all options'  and then type 'D1' in the search field. The search result list will then show the 'Standard_D1' size. Select it.

Note that we're selecting the smallest possible virtual machine size for this training in order to keep daily costs as low as possible. 

If you want, you can also select the Dedicated priority, GPU virtual machine type and then enter 'NC6' in the search field. This will display the 'Standard_NC6' size. This virtual machine will give you better performance but at a higher cost.

Now click the blue Next button.

![Compute setup step 2](./assets/new/image13.png)

Now we need to provide the following:

* The name of the new compute cluster. Give it a nice name like 'techionista_ccl'. 
* Minimum number of nodes. Set this to zero to ensure that all virtual machines in the cluster shut down when we are not training models.
* Maximum number of nodes. Set this to 2 so that during peak load we have two virtual machines running.
* Idle seconds before shut down. Set this to 120 seconds. If we are not using our cluster for more than two minutes, it will automatically shut down. 

Click the blue Create button to set up the cluster. 

![Compute setup step 2](./assets/new/image15.png)

After a couple of minutes the compute cluster will be running. You should see a green icon and the text 'Succeeded' in the Compute Cluster tab:

![Azure ML Compute overview](./assets/new/image17.png)

Congratulations! Your Azure Machine Learning Studio is now fully operational. We will use these compute resources in the upcoming course assignments to build, train, and run several cool machine learning models. 
 