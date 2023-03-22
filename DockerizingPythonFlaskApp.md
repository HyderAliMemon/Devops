Docker was created to address the problem of application portability and consistency across different environments. Prior to Docker, developers had to deal with issues such as differences in operating systems, dependencies, and configurations between development and production environments, leading to compatibility issues and deployment problems.

Docker has revolutionized the way we develop, deploy, and manage applications. With Docker, you can package an application and its dependencies into a container, which can then run consistently across different environments. This makes it easier to ship, scale, and update applications, as well as to isolate them from other applications and the underlying system.

If you’re a Python developer, you can leverage Docker to streamline your workflow and make your applications more portable and reproducible. Dockerizing your Python application involves creating a Docker image that contains your Python code, dependencies, and any required configuration. You can then use this image to run your application in any Docker environment, such as your local machine, a remote server, or a cloud provider.

In this blog post, we’ll walk you through the process of Dockerizing a Python application step by step. We’ll cover the basic Docker commands that you need to know, such as building an image, running a container, and publishing an image to a registry.

Whether you’re new to Docker or have some experience with it, this comprehensive guide will help you Dockerize your Python application like a pro. So, let’s get started!

<h2>Simple Python Flask Application</h2>

This is simple python flask app which will print Hello, World! on the website.</br>
<hr>
from flask import Flask</br>
app = Flask(__name__)</br>
@app.route("/")</br>
def home():</br>
    return "Hello, world!"</br>
if __name__ == "__main__":</br>
    app.run(host="0.0.0.0", port=5000)
    
    
<h2>Docker File</h2>
<hr>
Now we will create a docker file for above python flask app. In which we will include all the dependencies required to run this app.<br>

FROM python:3-alpine3.15</br>
WORKDIR /app</br>
COPY . /app</br>
RUN pip install flask</br>
EXPOSE 5000</br>
CMD python ./file.py</br>

<h2>Creating an image of docker file</h2>
After writing docker file, the first to do is to create an image using docker build command.<br>

#hyder/python-flask-app is my image name where as -t is used for tag<br>
<br>
<b>docker build -t hyder/python-flask-app . </b>
<br>
<h2>Checking image creation using docker desktop</h2>
Once you have run the build command for your Docker image, the image creation process will begin. If the build is successful, you can verify that the image has been created by running the “docker images” command in your terminal. This command will display a list of all the Docker images that have been created on your system.

In addition to the command line, you can also verify that the image has been created by checking the Docker Desktop interface.

#command to check created images
docker images

Flask app image on docker desktop
Running container using Docker
After creating image, now you can run images in container using following docker command:

"""
this command runs image in a container where 3000:5000 is port mapping
means you 5000 port application will run on 3000 port on local host. Where as 
-d and -p are tags
"""

<b>docker container run -d -p 3000:5000 hyder/python-flask-app</b><br>

After using above command, there are two ways to verify if container is running.

Using command:

Running container using docker container ls<br>

2. Using Docker Desktop:


Running Container on Docker Desktop<br>

<h2>Stop Running Container</h2>
<br>
The running container can also be stopped by using following command:
<br>
#in containerid you can also mention just starting 4 characters to stop container<br>
docker container stop containerid<br>
<h2>Pulling image from docker hub</h2>
docker pull is a command in the Docker ecosystem used to download Docker images from a remote registry. Docker images are pre-built packages containing everything needed to run a particular application or service. By using docker pull, you can easily download these images and use them to build and run your own containers. You can use following command to pull container from docker hub.


#where image is the name of image and tag is the image tag<br>
<br>
<b>docker pull image:tag</b>

#Example: To pull the image that we created we can use following command<br>

<b>docker pull hyder/python-flask-app</b> <br>

#as we haven't use any tag so it will pull image automatically
<h2>Pushing image on docker hub</h2>
The Docker push command is used to upload or push a local Docker image to a registry, such as Docker Hub or a private registry. This command allows users to share their images with others, deploy them to remote servers, or store them for future use. By pushing an image to a registry, it becomes available for other users to download and use, making it a crucial step in the Docker workflow. The push command requires the user to specify the image name and tag, and authenticate with the registry if necessary.
<br>
#command to push image to docker hub. where image is the name of image.<br>
<b>docker push image:tag</b>
