In today’s world, distributed systems have become increasingly complex, and the traditional monolithic architecture no longer meets the demands of modern applications. Microservices architecture has become the new norm, where applications are composed of loosely coupled, independently deployable services that communicate with each other over a network. However, as the number of microservices grows, managing service-to-service communication becomes more complex, leading to challenges such as service discovery, load balancing, and failure recovery. In response, service mesh technology has emerged as a powerful tool for solving these issues. In this blog post, we will explore the concept of service mesh, its importance, and how to install Linkerd, a popular service mesh implementation.

What is Service Mesh?
A service mesh is a layer of infrastructure that manages traffic between microservices in a distributed system. It provides a transparent, configurable, and secure way for services to communicate with each other by abstracting away the underlying network and infrastructure complexity. A service mesh is typically composed of two main components: the data plane and the control plane.

The data plane is responsible for the actual forwarding of traffic between services. It is made up of a set of proxies, which are deployed alongside each microservice, and are responsible for intercepting traffic and forwarding it to the correct destination. Proxies also implement features such as traffic shaping, load balancing, and circuit breaking.

The control plane is responsible for managing the configuration and behavior of the proxies in the data plane. It provides features such as service discovery, routing, security, and observability. The control plane can be managed by a centralized platform or by each individual service.

<h1>Why Use Service Mesh?</h1>
Service mesh provides several benefits to organizations that are running microservices-based applications. Here are some of the key benefits:

Scalability: Service mesh provides a scalable infrastructure for microservices communication by abstracting away the complexity of service-to-service communication.
Resilience: Service mesh provides resilience by implementing features such as circuit breaking, health checks, and retries.
Security: Service mesh provides security by implementing features such as mTLS, RBAC, and access control policies.
Observability: Service mesh provides observability by collecting and aggregating metrics, logs, and traces from the proxies.
Simplified Management: Service mesh provides a simplified way of managing microservices communication by abstracting away the underlying complexity of network and infrastructure.
Service Mesh Implementations
There are several service mesh implementations available, each with its own unique features and benefits. Here are some of the popular service mesh implementations:

Istio: Istio is an open-source service mesh that provides features such as traffic management, security, and observability.
Linkerd: Linkerd is a lightweight, open-source service mesh that provides features such as automatic mTLS encryption, traffic management, and observability.
Consul: Consul is a service mesh and service discovery platform that provides features such as service discovery, traffic management, and security.
Getting Started with Service Mesh
Let's install Linkerd and mesh the services.

Step 1: Setup
Before proceeding, it’s important to ensure that you have the ability to run Kubectl command on your local machine. If you encounter issues with running Kubectl, there is an alternative option available if you have Docker Desktop installed. By enabling the Kubernetes option and changing the Kubernetes context to “docker-desktop,” you can run Kubectl command via Docker Desktop. This step is crucial in setting up a successful installation and configuring the necessary tools for this process.

Step 2: Installing Linkerd CLI
After making sure that kubectl is working fine. You need to install Linkerd CLI to interact with your Linkerd deployment.

curl -sL run.linkerd.io/install | sh
In the event that an error occurs after running the aforementioned command, it may be necessary to install the latest version of Linkerd on a Windows platform. This can be done by downloading the latest version from GitHub and renaming the executable file to “linkerd.” Once this is complete, the file path must be added to the environment variable PATH. It’s important to follow these steps to ensure that Linkerd is installed correctly and can be utilized to its full potential.

Once Installed, verify the CLI is running correctly with:

linkerd version
Step 3: Validate Kubernetes Cluster
To check that your cluster is ready to install Linkerd, run:

linkerd check --pre

Status check result
If there are any checks that do not pass, make sure to resolve errors before proceeding.

Step 4: Install Linkerd onto your cluster
Now that you have the CLI running locally and a cluster that is ready to go, it’s time to install Linkerd on your Kubernetes cluster. To do this, run:

linkerd install --crds | kubectl apply -f -
followed by:

linkerd install | kubectl apply -f -
If there is any docker container runtime error after using the above command, then use this command instead:

linkerd install --set proxyInit.runAsRoot=true | kubectl apply -f -
These commands will install Linkerd Control Plane. To verify, if control plane is installed correctly run this command:

linkerd check
If all the checks pass, that mean the control plane is installed on your cluster.

Step 5: Install viz extension
To take a closer look at what linkerd is actually doing. We need to install extensions to monitor linkerd functionalities. To do so run the following command:

linkerd viz install | kubectl apply -f -
Once you’ve installed the extension, let’s validate everything one last time:

linkerd check
With the control plane and extensions installed and running, we’re now ready to explore Linkerd! Access the dashboard with:

linkerd viz dashboard &
Step 6: Install demo app
Now that you have successfully installed Linkerd, it’s time to see it in action with an application. The demo application we will be using is called Emojivoto, which is a straightforward Kubernetes application that enables users to vote for their favorite emojis through a combination of gRPC and HTTP calls.

To install Emojivoto into the emojivoto namespace, simply run the following command:

curl -sL https://run.linkerd.io/emojivoto.yml \ | kubectl apply -f -

Emojivoto app running on cluster
Let’s look at Emojivoto in its natural state before we mesh it. In order to direct our browser to it, we will accomplish this by forwarding traffic to its web-svc service. Forward web-svc locally to port 8080 by running:

kubectl -n emojivoto port-forward svc/web-svc 8080:80
Step 7: Meshing The App
Now that Emojivoto has been installed and is operational, the next step is to mesh it by incorporating Linkerd’s data plane proxies. To mesh your Emojivoto application, run the following command:

kubectl get -n emojivoto deploy -o yaml | linkerd inject - | kubectl apply -f -
Congratulations! You’ve now added Linkerd to an application! Just as with the control plane, it’s possible to verify that everything is working the way it should on the data plane side. Check your data plane with:

linkerd -n emojivoto check --proxy
Result:
Finally! All the services in emojivoto have been meshed using Linkerd.


Conclusion
In this blog, we covered the basics of service mesh technology, why it’s used, and how Linkerd is installed and meshed with a demo application. With these steps, you should now have a solid foundation in service mesh and be ready to explore the many powerful capabilities that Linkerd has to offer.

We hope that this guide has been informative and helpful in your journey to better understand service mesh technology.
