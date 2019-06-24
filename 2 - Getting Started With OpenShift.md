Source: [https://learn.openshift.com/introduction/getting-started/][1]

# Command Line Interface
The command line tool that we will be using as part of this training is called the oc tool. This tool is written in the Go programming language and is a single executable that is provided for Windows, OS X, and the Linux Operating Systems.

# Web Console
OpenShift also provides a feature rich Web Console that provides a friendly graphical interface for interacting with the platform.

# REST API
Both the command line tool and the web console actually communicate to OpenShift via the same method, the REST API. Having a robust API allows users to create their own scripts and automation depending on their specific requirements. For detailed information about the REST API, check out the official documentation at: https://docs.openshift.org/latest/rest_api/index.html

During this training, you will be using both the command line tool and the web console. However, it should be noted that there are plugins for several integrated development environments as well. For example, to use OpenShift from the Eclipse IDE, you would want to use the official https://tools.jboss.org/features/openshift.html plugin.

# Step 1 - Exploring The Command Line
The OpenShift CLI is accessed using the command oc. From here, you can administrate the entire OpenShift cluster and deploy new applications.

The CLI exposes the underlying Kubernetes orchestration system with the enhancements made by OpenShift. Users familiar with Kubernetes will be able to adapt to OpenShift quickly. The CLI is ideal in situations where you are:

1) Working directly with project source code.
2) Scripting OpenShift operations.
3) Restricted by bandwidth resources and cannot use the web console.

For this section, our task is going to be creating our first project.

# What is a project? Why does it matter?
The goal of this scenario is to get a project created and running, which you'll be doing with the web console in the next section.

OpenShift is often referred to as a container application platform in that it is a platform designed for the development and deployment of containers.

To contain your application, we use projects. The reason for having a project to contain your application is to allow for controlled access and quotas for developers or teams.

More technically, it's a visualization of the Kubernetes namespace based on the developer access controls.

# Command Line Interface (CLI)
In this course, we're not focusing on CLI, but we want you to be aware of it in case using the command line is your thing. Check out our other courses which go into the use of the CLI in more depth. Now, we're just going to practice logging in so you can get some experience with how the CLI works.

# Log in
1. Navigate to https://cluster.agile.eat.okd.secureworks.net
2. Log in with your `CORP` credentials
3. Click on your username in the upper right-hand corner of the web portal, and select `Copy Login Command`
4. Paste the command into your shell
**NOTE:** The login command is a password. Treat it as such!

# Create a Project for yourself
```bash
oc new-project <username>
```
Note that as you do this in the shell, the web console automatically updates. This is common throughout OpenShift.

# Exercise: Deploying your first Image
The simplest way to deploy an application in OpenShift is to take an existing Docker-formatted image and run it. We are going to use the OpenShift web console to do this, so ensure you have the OpenShift web console open and that you are in the project called myproject.

The OpenShift web console provides various options to deploy an application to a project. For this lab we are going to use the Deploy Image method. As the project is empty at this point, the Overview page should display a prominent Add to Project button.

![][image-1]
Selecting the Add to Project button you should be presented with the options of Browse Catalog, Deploy Image and Import YAML/JSON. Choose the Deploy Image tab.

![][image-2]
You could also have selected the Add to Project drop down menu in the top menu bar for the project and selected Deploy Image to go direct to the required tab.

Within the Deploy Image tab, choose the Image Name option. This will be used to reference an existing Docker-formatted image hosted on the Docker Hub Registry. For the name of the image enter:

openshiftroadshow/parksmap-katacoda:1.0.0

and press enter, or click on the magnifying glass icon to the right of the text entry box.

![][image-3]
At this point OpenShift will pull down and display key information about the image and the pending deployment, as well as populate the Name field with parksmap-katacoda. This name will be what is used for your application and the various components created which relate to it. Leave this as the generated value as steps given in this and subsequent labs will use this name.

You are ready to deploy the existing Docker-formatted image. Hit the blue Create button at the bottom of the screen and in the subsequent page click the Continue to overview link. This should bring you back to the Overview page where summary information about the application you just deployed will be displayed. If only the summary for the deployment is shown on the Overview page, select the arrow to the left of the deployment name, to expand the full detail.

![][image-4]
These are all the steps you need to run to get a "vanilla" Docker-formatted image deployed on OpenShift. This should work with any Docker-formatted image that follows best practices, such as defining the port any service is exposed on, not needing to run specifically as the root user or other dedicated user, and which embeds a default command for running the application.

# Step 4 - Scaling Your Application
Let's scale our application up to 2 instances of the pods. You can do this by clicking the "up" arrow next to the Pod in the OpenShift web console on the overview page.

![][image-5]
To verify that we changed the number of replicas, click the pods number in the circle next to the arrows. You should see a list with your pods similar to the following:

![][image-6]
You can see that we now have 2 replicas.

Overall, that's how simple it is to scale an application (Pods in a Service). Application scaling can happen extremely quickly because OpenShift is just launching new instances of an existing image, especially if that image is already cached on the node.

Application "Self Healing"
Because OpenShift's DeploymentConfigs are constantly monitoring to see that the desired number of Pods is actually running, you might also expect that OpenShift will "fix" the situation if it is ever not right. You would be correct!

Since we have two Pods running right now, let's see what happens if we "accidentally" kill one.

On the same page where you viewed the list of pods after scaling to 2 replicas, open one of the pods by clicking its name in the list.

In the top right corner of the page, there is Actions tab. When opened, there is the Delete action.

![][image-7]
Click it! And confirm the dialog. And you will be taken back to the page listing pods, however this time, there are three pods.

![][image-8]
The pod that we deleted is terminating, i.e. it is being cleaned up. And a new pod was created, because OpenShift will always make sure, that if one pod dies, there is going to be new pod created to fill its place.

# Exercise: Scale Down
Before we continue, go ahead and scale your application down to a single instance. It's as simple as clicking the down arrow on the Overview page.

Step 5 - Routing HTTP Requests
Services provide internal abstraction and load balancing within an OpenShift environment, as sometimes clients (users, systems, devices, etc.) outside of OpenShift need to access an application. The way that external clients are able to access applications running in OpenShift is through the OpenShift routing layer. The data object behind that layer is a Route.

The default OpenShift router (HAProxy) uses the HTTP header of the incoming request to determine where to proxy the connection. You can optionally define security, such as TLS, for the Route. If you want your Services, and, by extension, your Pods, to be accessible to the outside world, you need to create a Route.

Task: Creating a Route
Fortunately, creating a Route is a pretty straight-forward process. You just click the "Create Route" link displayed against the application on the Overview page.

![][image-9]

There is no need to change the default settings in the Route creation form.

![][image-10]
Once you click Create, the Route will be created and displayed in the Overview page.

![][image-11]
We can also get the list of all the existing Routes by clicking the Applications-\>Routes menu:

![][image-12]
Currently the list of Routes will only display the one we just created.

![][image-13]
In this list we will be able to see the details associated with the route, like the hostname, the service, the port the route is exposing, and details on the TLS security for the route, if any.

You can always click on the Route name in this list to modify an existing Route.

Now that we know how to create a Route, let's verify that the application is really available at the URL shown in the web console. Click the link and you will see:

![][image-14]

[1]:	https://learn.openshift.com/introduction/getting-started/

[image-1]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/3add-to-empty-project.png
[image-2]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/3add-to-project-options.png
[image-3]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/3deploy-image-parksmap.png
[image-4]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/3deploy-image-parksmap.png
[image-5]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/4scaling-arrows.png
[image-6]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/4scaling-pods.png
[image-7]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/4scaling-actions.png
[image-8]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/4scaling-terminating.png
[image-9]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/5no-route.png
[image-10]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/5create-route.png
[image-11]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/5route-created.png
[image-12]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/5routes-menu.png
[image-13]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/5routes-list.png
[image-14]:	https://katacoda.com/embed/openshift/courses/assets/introduction/getting-started/5parksmap-empty.png