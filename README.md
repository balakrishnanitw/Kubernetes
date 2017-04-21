# Kubernetes
Create a Bluemix Account
Signup for Bluemix, or use an existing account.

Download and install the Cloud-foundry CLI tool

If you have not already, [download node.js 6.7.0 or later][https://nodejs.org/download/] and install it on your local machine.

Clone the app to your local environment from your terminal using the following command

git clone https://github.com/IBM-Bluemix/bluechatter.git
cd into the bluechatter folder that you cloned
cd bluechatter
Edit the manifest.yml file and change the application host to something unique. The host you use will determinate your application url initially, e.g. <host>.mybluemix.net.

Connect and login to Bluemix
<br>
$ cf login -a https://api.ng.bluemix.net
</br>
Create a Redis service for the app to use, we will use the RedisCloud service.
$ cf create-service rediscloud 30mb redis-chatter
Push the application
cf push
Done, now go to the staging domain(<host>.mybluemix.net.) and see your app running.

Scaling Your Cloud Foundry Application

Since we are using Redis to send chat messages, you can scale this application as much as you would like and people can be connecting to various servers and still receive chat messages. We will be looking on how to scale the application runtime instances for when needed, to do this we are going to look at the manual scaling command or use the Auto-Scaling service to automatically increase or decrease the number of application instances based on a policy we set it.

Manual Scaling

Manually scale the application to run 3 instances
$ cf scale my-blue-chatter-app-name -i 3
Then check your that all your instances are up and running.
 $ cf app my-blue-chatter-app-name
Now switch over to your staging domain(<host>.mybluemix.net.) to see your running application. Note, you know which instance you are connecting to in the footer of the form. If you have more than one instance running chances are the instance id will be different between two different browsers.

Auto Scaling

It's good to be able to manually scale your application but Manual scaling wont work for many cases, for that reason we need to setup a Auto-Scaling to automatically scale our application for when needed. To learn more on Auto-Scaling checkout the blog post Handle the Unexpected with Bluemix Auto-Scaling for detailed descreption on Auto-Scaling.
2 Docker Deployment Approach

Here we are going to look on how to deploy the BlueChatter application on a Docker container where it will be running on IBM Bluemix. We will then look on how to scale your Docker container on Bluemix to scale your application where needed. First, let's look at running the BlueChatter application inside a Docker container locally on your machine. Next, we will deploy the container to Bluemix and then scale it for when needed. Let's get started and have fun.

2_1 Setup

Create a Bluemix Account
Signup for Bluemix, or use an existing account.

Download and install the Cloud-foundry CLI tool

Install Docker using the Docker installer, once installation completed, test if docker installed by typing the "docker" command in your terminal window, if you see the list of docker commands, then you are ready to go.

Install the IBM Bluemix Container Service plug-in to execute commands to IBM Bluemix containers from your terminal window. Install Container Service plug-in by running this command if on OS X.

$ cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-mac
If you have not already, [download node.js 6.7.0 or later][https://nodejs.org/download/] and install it on your local machine.

Clone the app to your local environment from your terminal using the following command

git clone https://github.com/IBM-Bluemix/bluechatter.git
cd into the bluechatter folder that you cloned
cd bluechatter
2_2 Build & run container locally

Build your docker container
$ docker-compose build
Start your docker container
$ docker-compose up
Done, now go to http://localhost to see your app running if you are on a Mac Linux.

On Windows, you will need the IP address of your Docker Machine VM to test the application. Run the following command, replacing machine-name with the name of your Docker Machine.
$ docker-machine ip machine-name
Now that you have the IP go to your favorite browser and enter the IP in the address bar, you should see the app come up. (The app is running on port 80 in the container.)

2_3 Run container on Bluemix

Before running the container on Bluemix, I recommend you to checkout the Docker container Bluemix documentation to better understand the steps below. Please review the documentation on Bluemix before continuing.

Download and install the Cloud-foundry CLI tool if haven't already.

Install the IBM Bluemix Container Service plug-in to execute commands to IBM Bluemix containers from your terminal window. Install Container Service plug-in by running this command if on OS X.

$ cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-mac
If you are on Linux or windows then find the installation command here

Login to Bluemix with your Bluemix email and password
$ cf login -a api.ng.bluemix.net
Login to the IBM Containers plugin to work with your docker images that are on Bluemix
$ cf ic login
After you have logged in you can build an image for the BlueChatter application on Bluemix. From the root of BlueChatter application run the following command replacing namespace with your namespace for the IBM Containers service. (If you don't know what your namespace is run $ cf ic namespace get.)
$ cf ic build -t namespace/bluechatter ./
The above build command will push the code for Bluechatter to the IBM Containers Docker service and run a docker build on that code. Once the build finishes the resulting image will be deployed to your Docker registry on Bluemix. You can verify this by running

The above build command will push the Bluechatter application to the IBM Containers Docker service and run a docker build on that code. Once the build finishes the resulting image will be deployed to your Docker registry on Bluemix. You can verify this by running

$ cf ic images
You should see a new image with the tag namespace/bluechatter listed in the images available to you. You can also verify this by going to the catalog on Bluemix, in the containers section you should see the BlueChatter image listed.

Our BlueChatter application is using the Redis cloud service to store in memory the chat communication, let's go ahead and use the Redis service. We can create a service on Bluemix using the Bluemix UI or the terminal using the command below. Enter this command in your terminal to create the Redis service:
$ cf create-service rediscloud 30mb redis-chatter
Steps to be done on Bluemix UI

Now we need to switch over to Bluemix UI and complete the steps required to have our Docker image running inside a private docker container repository on Bluemix.
