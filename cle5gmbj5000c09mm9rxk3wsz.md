# Terraform Challenge-2

Welcome to the terraform challenge series. In this challenge, we will implement a simple LAMP stack using terraform and docker.

> Signup to [KodeKloud](https://kodekloud.com/) for performing this challenge. It's free!!!

# Architecture Diagram

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676451708545/43c1c384-3556-48f4-b7c8-1a8047fe8615.png align="center")

# Challenge

We will implement a simple LAMP stack using terraform and docker.

**The requirements in detail:**

* Install terraform binary version=1.1.5 on iac-server
    
* The Docker provider has already been configured using the kreuzwerker/docker provider. Check out the provider. tf given at /root/code/terraform-challenges/challenge2
    

## Create a terraform resource named `php-httpd-image` for building a docker image with the following specifications:

* Image name: `php-httpd: challenge`
    
* Build context: `lamp_stack/php_httpd`
    
* Labels: `challenge: second`
    

## Create a terraform resource named `mariadb-image` for building a docker image with the following specifications:

* Image name: `mariadb: challenge`
    
* Build context: `lamp_stack/custom_db`
    
* Labels: `challenge: second`
    

## Define a terraform resource `php-httpd` for creating a docker container with the following specifications:

* Container Name: `webserver`
    
* Hostname: php-httpd
    
* Image used: php-httpd: challenge
    
* Attach the container to the network `my_network`
    
* Publish a container's port(s) to the host:  
    Host port: `0.0.0.0:80`  
    containerPort: `80`
    
* Labels: `challenge: second`
    
* Create a `volume` with host\_path `/root/code/terraform-challenges/challenge2/lamp_stack/website_content/` and container\_path `/var/www/html` within the webserver container.
    

## Define a terraform resource `phpmyadmin` for docker container with the following configurations:

* Container Name: `db_dashboard`
    
* Image Used: `phpmyadmin/phpmyadmin`
    
* Hostname: `phpmyadmin`
    
* Attach the container to the network `my_network`
    
* Publish a container's port(s) to the host:  
    Host port: `0.0.0.0:8081`  
    containerPort: `80`
    
* Labels: `challenge: second`
    
* Establish link-based connectivity between `db` and `db_dashboard` containers (Deprecated)
    
* Explicitly specify a dependency on `mariadb` terraform resource
    

## Define a terraform resource `mariadb` for docker container with the following configurations:

* Container Name: `db`
    
* Image Used: `mariadb: challenge`
    
* Hostname: `db`
    
* Attach the container to the network `my_network`
    
* Publish a container's port(s) to the host:  
    Hostport: `0.0.0.0:3306`  
    containerPort: `3306`
    
* Labels: `challenge: second`
    
* Define environment variables inside `mariadb` resource:  
    `MYSQL_ROOT_PASSWORD=1234`  
    `MYSQL_DATABASE=simple-website`
    
* Attach volume `mariadb-volume` to `/var/lib/mysql` a directory within db container.
    

## Create a terraform resource named `mariadb_volume` creating a docker volume with `name=mariadb-volume`

# Solution:

## 1\. Install terraform binary version=1.1.5 on iac-server

```plaintext
curl -L -o /tmp/terraform_1.1.5_linux_amd64.zip https://releases.hashicorp.com/terraform/1.1.5/terraform_1.1.5_linux_amd64.zip
unzip -d /usr/local/bin /tmp/terraform_1.1.5_linux_amd64.zip
```

## 2\. Check out the provider. tf

```plaintext
cd /root/code/terraform-challenges/challenge2
cat provider.tf
```

Let's initialize the provider now.

```plaintext
terraform init
```

> You should now refer to the documentation for this provider. Go to the [Terraform Registry](https://registry.terraform.io/) and paste `kreuzwerker/docker` into the search bar.

## 3\. Create a terraform resources

| Resource Name | Provider Documentation |
| --- | --- |
| php-httpd-image, mariadb-image | [docker-image](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/image) |
| mariadb\_volume | [docker-volume](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/volume) |
| private\_network | [docker-network](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/network) |
| mariadb, php-httpd, phpmyadmin | [docker-container](https://registry.terraform.io/providers/kreuzwerker/docker/latest/docs/resources/container) |

### main.tf

```plaintext
#Terraform resource named php-httpd-image
resource "docker_image" "php-httpd-image" {
  name = "php-httpd:challenge"
  build {
    path = "lamp_stack/php_httpd"
    tag  = ["php-httpd:challenge"]
    label = {
      challenge : "second"
    }
  }
}
#Terraform resource named mariadb-image
resource "docker_image" "mariadb-image" {
  name = "mariadb:challenge"
  build {
    path = "lamp_stack/custom_db"
    tag  = ["mariadb:challenge"]
    label = {
      challenge : "second"
    }
  }
}
#Terraform resource php-httpd for creating docker container
resource "docker_container" "php-httpd" {
  name     = "webserver"
  image    = docker_image.php-httpd-image.name
  hostname = "php-httpd"
  networks_advanced {
    name = docker_network.private_network.id
  }
  ports {
    internal = 80
    external = 80
    ip       = "0.0.0.0"
  }
  labels {
    label = "challenge"
    value = "second"
  }
  volumes {
    container_path = "/var/www/html"
    host_path      = "/root/code/terraform-challenges/challenge2/lamp_stack/website_content/"
  }
}
#Terraform resource phpmyadmin for docker container
resource "docker_container" "phpmyadmin" {
  name     = "db_dashboard"
  image    = "phpmyadmin/phpmyadmin"
  hostname = "phpmyadmin"
  networks_advanced {
    name = docker_network.private_network.id
  }
  ports {
    internal = "80"
    external = "8081"
    ip       = "0.0.0.0"
  }
  labels {
    label = "challenge"
    value = "second"
  }
  links = [
    docker_container.mariadb.name
  ]
  depends_on = [
    docker_container.mariadb
  ] 
}
#Terraform resource named private_network
resource "docker_network" "private_network" {
  name       = "my_network"
  attachable = true
  labels {
    label = "challenge"
    value = "second"
  }
}
#Terraform resource mariadb for creating docker container
resource "docker_container" "mariadb" {
  name     = "db"
  image    = docker_image.mariadb-image.name
  hostname = "db"
  networks_advanced {
    name = docker_network.private_network.id
  }
  ports {
    internal = 3306
    external = 3306
    ip       = "0.0.0.0"
  }
  labels {
    label = "challenge"
    value = "second"
  }
  env = [
    "MYSQL_ROOT_PASSWORD=1234",
    "MYSQL_DATABASE=simple-website"
  ]
  volumes {
    container_path = "/var/lib/mysql"
    volume_name    = docker_volume.mariadb_volume.name
  }
}
#Terraform resource named mariadb_volume
resource "docker_volume" "mariadb_volume" {
  name = "mariadb-volume"
}
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner.

Hope you learned something new today :)

Signup to [KodeKloud](https://kodekloud.com/) for performing this challenge.

<iframe src="https://giphy.com/embed/3o7buf60fd2oOo7DGM" class="giphy-embed" width="480" height="270"></iframe>

[via GIPHY](https://giphy.com/gifs/hotones-first-we-feast-hot-ones-3o7buf60fd2oOo7DGM)