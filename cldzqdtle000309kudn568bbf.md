# Terraform Challenge-1

For these challenges, you will start with deploying a simple web app on an existing Kubernetes cluster and then move on to more complicated challenges involving provisioning resources and entire infrastructure stacks in the AWS cloud.

> Signup to [KodeKloud](https://kodekloud.com/) for performing this challenge. It's free!!!

# Architecture Diagram

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676451598542/c71b2ec7-42ca-4c55-b74b-b65fa5da9081.png align="center")

# Challenge

In this challenge, we will deploy several Kubernetes resources using Terraform. If you don't know Kubernetes, this will be somewhat confusing as the resource schema only makes sense if you understand Kubernetes.

Utilize `/root/terraform_challenge` a directory to store your Terraform configuration files.

**The requirements in detail:**

* Terraform version: `1.1.5` installed on controlplane?
    

## Configure `terraform and provider` settings within the `provider.tf` file with the following specifications:

* Configure terraform to use `hashicorp/kubernetes` provider.
    
* Specify the provider's local name: `kubernetes`
    
* Provider version: `2.11.0`
    
* Configure Kubernetes provider with the path to your `kubeconfig` file: `/root/.kube/config`
    

## Create a terraform resource `frontend` for Kubernetes deployment with the following specs:

* Deployment Name: `frontend`
    
* Deployment Labels = `name: frontend`
    
* Replicas: `4`
    
* Pod Labels = `name: webapp`
    
* Image: `kodekloud/webapp-color:v1`
    
* Container name: `simple-webapp`
    
* Container port: `8080`
    

## Create a terraform resource `webapp-service` for Kubernetes service with the following specs:

* Service name: `webapp-service`
    
* Service Type: `NodePort`
    
* Port: `8080`  
    NodePort: `30080`
    

# Solution:

## 1\. Check Terraform

```plaintext
which terraform
```

> Nothing! Therefore we must install it. Note that unzip is also not installed, and we need that too!

```plaintext
apt update
apt install unzip -y
curl -L -o /tmp/terraform_1.1.5_linux_amd64.zip https://releases.hashicorp.com/terraform/1.1.5/terraform_1.1.5_linux_amd64.zip
unzip -d /usr/local/bin /tmp/terraform_1.1.5_linux_amd64.zip
```

## 2\. Configure terraform and provider settings within the provider.tf file

You should now refer to the documentation for this provider. Go to the [Terraform Registry](https://registry.terraform.io/) and paste `hashicorp/kubernetes` it into the search bar. This will give you the latest version, so adjust the URL in your browser to `2.11.0`

Click on the **USE PROVIDER** button for the configuration block. Copy this, and use `vi` it to create provider.tf. Paste in and adjust as per the question requirements.

### provider.tf

```plaintext
terraform {
  required_providers {
    kubernetes = {
      source = "hashicorp/kubernetes"
      version = "2.11.0"
    }
  }
}

provider "kubernetes" {
  config_path    = "/root/.kube/config"
}
```

Now we can initialize the provider

```plaintext
terraform init
```

## 3\. Create terraform resources frontend & webapp-services for Kubernetes deployment & Kubernetes services

Refer to the provider documentation for [kubernetes\_deployment](https://registry.terraform.io/providers/hashicorp/kubernetes/2.11.0/docs/resources/deployment) & [kubernetes\_service](https://registry.terraform.io/providers/hashicorp/kubernetes/2.11.0/docs/resources/service)

### main.tf

```plaintext
resource "kubernetes_deployment" "frontend" {
  metadata {
    name = "frontend"
    labels = {
      name = "frontend"
    }
  }
  spec {
    replicas = 4
    selector {
      match_labels = {
        name = "webapp"
      }
    }
    template {
      metadata {
        labels = {
          name = "webapp"
        }
      }
      spec {
        container {
          image = "kodekloud/webapp-color:v1"
          name  = "simple-webapp"
          port {
            container_port = 8080
          }
        }
      }
    }
  }
}

resource "kubernetes_service" "webapp-service" {
  metadata {
    name = "webapp-service"
  }
  spec {
    selector = {
      name = "webapp"
    }
    port {
      port        = 8080
      target_port = 8080
      node_port   = 30080
    }

    type = "NodePort"
  }
}
```

## 4\. Deploy

```plaintext
terraform plan
terraform apply
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner.

Hope you learned something new today :)

Signup to [KodeKloud](https://kodekloud.com/) for performing this challenge.

<iframe src="https://giphy.com/embed/3o6Ztk950bvRtZLGk8" class="giphy-embed" width="480" height="270"></iframe>

[via GIPHY](https://giphy.com/gifs/southparkgifs-3o6Ztk950bvRtZLGk8)