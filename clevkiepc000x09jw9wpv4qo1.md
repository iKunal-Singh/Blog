---
title: "Microservice Architecture and System Design with Python & Kubernetes | Auth"
seoDescription: "Building Robust and Scalable Systems with Microservice Architecture: An In-Depth Exploration of System Design"
datePublished: Sun Mar 05 2023 15:48:21 GMT+0000 (Coordinated Universal Time)
cuid: clevkiepc000x09jw9wpv4qo1
slug: microservice-architecture-and-system-design-with-python-kubernetes-auth
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1678029703523/e5759b1e-c73e-4659-b989-5aa22f8edcb9.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1678031211892/4909bd62-cadf-47ea-8fca-08e3ab4f672d.png
tags: microservices, python, kubernetes, architecture, system-design

---

# Architecture 📐

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677949329156/f6510779-ffc8-4567-a454-3795b87f7ed1.png align="center")

# Tools and Languages Used 🧰

1. **Python**
    
2. **Docker**
    
3. **Kubernetes**
    
4. **MongoDB**
    
5. **MySQL**
    
6. **RabbitMQ**
    

# Working ❇️

* **The Client** uploads the video to be converted, the video will first hit our **API gateway**.
    
* **API gateway** will store the video in **MongoDB** and put a message in our **Queue**, letting downstream services know that there is a video to be processed on **MongoDB**.
    
* **The Converter service** will consume the message from the **Queue**. It will then get the **Video ID** from the message and pull that video from **MongoDB** convert it to mp3 and store the mp3 in **MongoDB** and then put a new message on the **Queue** to be consumed by the **Notification service**.
    
* **The Notification service** will send an email to clients letting them know the file is ready to be downloaded.
    
* **The Client** will use the **Video ID** acquired from the **Notification service** and his/her **JWT(JSON Web Token)** to make a request to the **API gateway** to download the mp3 then the **API gateway** will pull the mp3 from **MongoDB** and serve it to **The Client**.
    

# Auth Service 🔐

## [server.py](https://github.com/iKunal-Singh/Lucian-MASD/blob/master/auth/server.py) 🐍

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677912935329/a9d486f2-357c-4f76-99ed-3f8bab83f45e.png align="center")

* import JWT, datetime, os **\-&gt;** JWT is our actual **auth** **|** datetime is used to set **expiration** on our token **|** os is used so that we can use an **environment variable** to configure our **MySQL connection**.
    
* from flask import FLASK, request **\-&gt;** We are using **Flask** to create our **server**. **|** request.
    
* from flask\_mysqldb import MySQL **\-&gt;** Allows us to Query our MySQL database.
    
* server = Flask(\_\_name\_\_) **\-&gt;** This is going to **configure our server** so that requests to **specific routes** can interface with our code.
    
* mysql = MySQL(server) **\-&gt;** This **MySQL object** is going to make sure that our application can connect to our MySQL database and query the
    
    database.
    
* server.config\["MYSQL\_HOST"\] = os.environ.get("MYSQL\_HOST") **\-&gt;** Our **server object** has a **config attribute** which is essentially a dictionary that we can use to **store configuration variables**. **|** os.environ.get("MySQL") is going to get the MySQL host from our environment.
    

> This and following other server.configs is going to be the configuration for our application and the variables that we use to connect to our MySQL database.

* @ server.route("/login", methods=\["POST"\]) -&gt; This defines the **"/login" endpoint** for a **Flask server**. When an **HTTP POST** request is made to the "/login" endpoint,
    
    the following code will be executed.
    
* auth = request.authorization -&gt; **authorization** attribute provides the credentials from a basic **authentication scheme,** so when we send a request to the **login route,** we need to provide a basic **authentication scheme** that will contain essentially a **username and a password,** and the **request object** has an attribute that gives us access to that.
    
* if not auth: return "Missing Credentials", 401 -&gt; If the authorization information is missing, the code returns a message "Missing Credentials" and an HTTP status code 401 (Unauthorized).
    

> So the way login route is going to work is, auth service is going to have its own MySQL database and we're going to check a user table within that database and the user table should contain a username and password for the users that are trying to log in or trying to access the API .

* cur = mysql.connection.cursor() -&gt; This creates a database cursor using the MySQL connection created earlier in the code.
    
* res = cur.execute( "SELECT email, password FROM user WHERE email=%s", (auth.username,) ) -&gt; Using the cursor defined above we are executing an SQL query to retrieve the email and password from the user table with whatever email passed in the request and we are using email for our username.
    
* if \_\_name\_\_ == "\_\_main\_\_": -&gt; It is executed only if the script is run as a standalone program.
    
* server.run(host="0.0.0.0", port=5000) -&gt; we want our server to run on port "5000" and configure the host parameter to allow our application to listen to any IP address.
    

### Why host="0.0.0.0"? and What if it's not "0.0.0.0? ❓

In easy terms, if we don't set this host parameter like so the default is going to be localhost which means that the server is only accessible from our own computer or from localhost and our API wouldn't be available externally.

Briefly explain, any server is going to need an IP address to allow access from outside of the server so in our case the server is a Docker container and our application will be running within that container when we spin up our Docker container it will be given its own IP address we can then use that IP address to send requests to our Docker container but this alone isn't enough to enable our flask application to receive those requests we need to tell our flask application to listen on our container's IP address so that when request gets into our containers IP our application can receive those requests so this is where the host config comes in the play host is the server that is hosting our application in our case the server that is hosting our flask application is the docker container so we need to tell our flask app to listen on our Docker containers IP address but a Docker container's IP address is subject to change so instead of setting it to the static IP address of our Docker container we set it to this "0.0.0.0" IP address which is kind of like a wild card that tells our our flask app to listen on any and all of our Docker containers IP addresses and if we don't configure this it will default to just localhost which is the loopback address and localhost is only accessible from within the host therefore outside requests sent to our Docker container would never actually make it to our flask app because the loopback address isn't publicly accessible.

### [Authentication schemes](https://developer.mozilla.org/en-US/docs/Web/HTTP/authentication) Docs 📃

---

## [init.sql](https://github.com/iKunal-Singh/Lucian-MASD/blob/master/auth/init.sql)

We are creating a **USER** for our **auth service** and we are giving that user a username and a password then we are creating a **MySQL database** called **auth** which is going to be the database for our auth service and we are granting the **USER** privileges to the database and we are creating a table within that database called **user** not to be mistaken with the **USER** that we're creating within the MySQL database and the **user** table is used to store users that we want to give access to our API and we are creating an initial user which will go in our database and will have access to our **Gateway API.**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677912766846/c36fad4d-4f5e-4850-a6a4-0021e04446d6.png align="center")

### Basic commands to work with MySQL database 💽

* **show databases;** -&gt; Displays a list of all databases on the MySQL server.
    
* **use auth;** -&gt;Selects the auth database as the default database for subsequent commands.
    
* **show tables;** -&gt; Displays a list of all tables in the selected database.
    
* **describe user;** -&gt; Displays information about the columns in the user table, such as the name, data type, and constraints of each column.
    
* **select \* from user;** -&gt; Retrieves all columns and all rows from the `user` table.
    

---

## [Dockerfile](https://github.com/iKunal-Singh/Lucian-MASD/blob/master/auth/Dockerfile) 🐳

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677912654614/d721de7b-f673-4775-9eb0-84d21b127807.png align="center")

Each instruction in a Docker file results in a single new image layer being created which means that the following instructions image layer will be built on top of the previous instructions image layer. so that means the FROM instruction creates a layer and then the RUN instruction creates a new layer on top of the previous layer that was built from the FROM instruction and this continues until we reach the end of our Docker file.

---

## Manifest

All the codes inside this are our Kubernetes Configuration codes. Clicking on the name of the files will take you to its API reference.

[Kubernetes Documentation](https://kubernetes.io/)

[Kubernetes API reference](https://kubernetes.io/docs/reference/kubernetes-api/)

### [auth-deploy.yaml](https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/)

Deployment enables declarative updates for Pods and ReplicaSets.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677912515751/ca5764f1-140d-4d81-a5dc-00b5b19927c3.png align="center")

* **Line-7, spec:** The specification of the desired behavior of deployment.
    
* Line-8, replicas: Number of desired pods.
    
* Line-9, selector: Existing replica-set whose pods are selected by this will be the one affected.
    
* Line-12, strategy: Strategy to use to replace the existing pods with the new ones.
    
* Line-15, maxSurge: Maximum number of pods that can be scheduled above the desired number of pods.
    
* Line-16, template: Defines the pods that will be created from this pod template.
    
* Line-20, spec: The data pod should have when created from the template.
    
* Line-26, envFrom: List of sources to populate environment variable in the containers.
    

### [configmap.yaml](https://github.com/iKunal-Singh/Lucian-MASD/blob/master/auth/manifest/configmap.yaml)

Going to set environment variables within our container.

In other words, if we were to run the environment command within that container all of these variables and their values are going to be present within the container.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677912579032/53240777-6f0f-4652-bf72-497c37e6a942.png align="center")

* **MYSQL\_HOST: host.minikube.internal :-** We are using our MySQL server. So we need to reference that server from within our Kubernetes cluster and Minikube gives us a way to access our host, the Clusters hosts via this **host.mininkube.internal** because within the cluster we're kind of in our own isolated network. So since our MySQL server is just deployed to our local host from within the cluster we wouldn't be able to just use localhost we need to access the system that's hosting the cluster and that's what this **host.mininkube.internal** is for.
    

### [secret.yaml](https://github.com/iKunal-Singh/Lucian-MASD/blob/master/auth/manifest/secret.yaml)

A file that is used to store sensitive information for our Kubernetes applications. We use this file to keep our secrets hidden from public view and ensure the security of our applications.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678030319186/909e0e4d-30e4-4e5a-8f35-f1b5426fd80e.png align="center")

* **type: Opaque:-** Indicates that the data in the Secret object is arbitrary and unstructured. This is suitable for storing simple key-value pairs like we have in this example.
    

### [service.yaml](https://github.com/iKunal-Singh/Lucian-MASD/blob/master/auth/manifest/service.yaml)

We are creating a Service object in Kubernetes to expose a set of running pods as a network service that can be accessed by other containers or applications. It provides a stable IP address and DNS name for external clients to access the pods.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678030882802/1de9ae99-79f2-4be7-ab85-80eefd674d68.png align="center")

* **type: ClusterIP:-** The type of the Service is specified as ClusterIP, which means that this Service is only accessible from within the cluster.
    

# Summary 🏁

So, in summary, what you're doing here is deploying an auth service using Kubernetes. There are a few steps involved in this process:

1. Writing infrastructure code for auth deployment: First, you created a directory called "manifest" and wrote the necessary infrastructure code to deploy the auth service using Kubernetes. This code includes YAML files that specify the Kubernetes resources needed to deploy the service, such as deployment, a service, a config map, and a secret.
    
2. Writing the auth service code: In the main directory, you wrote the code for the actual auth service. This code is likely written in a programming language like Python or Go.
    
3. Creating a Docker image: To package the auth service code, you created a Dockerfile that specifies the dependencies and runtime environment needed for the service to run. You then built this Dockerfile into a Docker image that contains the auth service code.
    
4. Pushing the Docker image to a repository: Once the Docker image was built, you pushed it to a repository on the internet, such as Docker Hub or a private registry.
    
5. Pulling the Docker image and deploying it to Kubernetes: Within the infrastructure code in the manifest directory, you wrote code to pull the Docker image from the repository and deploy it to Kubernetes. This code uses the Kubernetes API to interface with the Kubernetes cluster and create the necessary resources for the auth service.
    
6. Applying the Kubernetes manifest files: To actually deploy the auth service, all you need to do is run the command "kubectl apply -f ./" from the manifest directory. This command tells Kubernetes to apply the manifest files in that directory, which in turn deploys the auth service.
    

Overall, this process involves writing code for both the auth service itself and the infrastructure needed to deploy it using Kubernetes. Docker is used to package the service code and deploy it consistently across different environments, while Kubernetes provides the tools to manage and scale the service in production.

# Important 👈

Thank you for reading this blog on **Microservice Architecture and System Design using Python and Kubernetes.** Over the next few weeks, I will be posting a 4-part series in which we will dive deeper into each of the services that we have built.

The first part of this series will focus on the [**Auth Service**](https://github.com/iKunal-Singh/Lucian-MASD/tree/master/auth)**,** which is responsible for handling user authentication and authorization. The second part will cover the [**Converter Service**](https://github.com/iKunal-Singh/Lucian-MASD/tree/master/converter), which is responsible for converting data from one format to another.

In the third part, we will explore the [**Gateway Service**](https://github.com/iKunal-Singh/Lucian-MASD/tree/master/gateway), which is responsible for routing requests to the appropriate service. Finally, in the fourth part, we will dive into the [**Notification Service**,](https://github.com/iKunal-Singh/Lucian-MASD/tree/master/notification) which is responsible for sending notifications to users.

We hope that this series will provide you with a comprehensive understanding of Microservice Architecture and System Design using Python and Kubernetes. Stay tuned for the upcoming posts!

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learned something new today :)

Any feedback for further improvement will be highly appreciated!

<iframe src="https://giphy.com/embed/v7TnlySWTSB7BFb5Q1" class="giphy-embed" width="480" height="422"></iframe>

[via GIPHY](https://giphy.com/gifs/nfl-bills-buffalo-dawson-knox-v7TnlySWTSB7BFb5Q1)