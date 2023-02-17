# Terraform Challenge-3

> Signup to [KodeKloud](https://kodekloud.com/) for performing this challenge. It's free!!!

# Architecture Diagram

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676649173401/dfccac97-3c22-4eeb-8155-d424d4119182.png align="center")

# Challenge

In this challenge, we will implement a simple EC2 instance with some preinstalled packages.

## **The requirements in detail:**

* Create a terraform key-pair `citadel-key` with key\_name `citadel`.
    
* Upload the public key [`ec2-connect-key.pub`](http://ec2-connect-key.pub) to the resource. You may use the `file function` to read the public key at `/root/terraform-challenges/project-citadel/.ssh`
    
* AMI: ami-06178cf087598769c, use variable named `ami`
    
* Region: eu-west-2, use variable named `region`
    
* Instance Type: m5.large, use variable named `instance_type`
    
* Elastic IP address attached to the EC2 instance
    
* Create a local-exec provisioner for the `eip` resource and use it to print the attribute called `public_dns` to a file `/root/citadel_public_dns.txt` on the iac-server
    
* Install Nginx on the citadel instance, and make use of the `user_data` argument.  
    Using the file function or by making use of the heredoc syntax, use the script called [`install-nginx.sh`](http://install-nginx.sh) as the value for the user\_data argument.
    

# Solution:

## 1\. Declare variables

### variables.tf

```plaintext
variable "ami" {
  type    = string
  default = "ami-06178cf087598769c"
}

variable "region" {
  type    = string
  default = "eu-west-2"
}

variable "instance_type" {
  type    = string
  default = "m5.large"
}
```

Let's initialize the provider now.

```plaintext
terraform init
```

## 2\. Create a terraform resources

| Resource Name | Provider Documentation |
| --- | --- |
| citadel-key | [aws\_key\_pair](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/key_pair) |
| citadel | [aws\_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance) |
| eip | [aws\_eip](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/eip) |

> Go to the [Terraform Registry](https://registry.terraform.io/). The AWS provider is on the front page.
> 
> The core documentation for the [file](https://www.terraform.io/language/functions/file) function.
> 
> The core documentation for [local-exec provisioner](https://www.terraform.io/language/resources/provisioners/local-exec)

### main.tf

```plaintext
#A terraform key-pair citadel-key with key_name citadel
resource "aws_key_pair" "citadel-key" {
  key_name   = "citadel"
  public_key = file("/root/terraform-challenges/project-citadel/.ssh/ec2-connect-key.pub")
}
#This step covers both the citadel and Nginx-script tasks.
resource "aws_instance" "citadel" {
  ami           = var.ami
  instance_type = var.instance_type
  key_name      = aws_key_pair.citadel-key.key_name
  user_data     = file("/root/terraform-challenges/project-citadel/install-nginx.sh")
}
#A local-exec provisioner for the eip resource
resource "aws_eip" "eip" {
  vpc      = true
  instance = aws_instance.citadel.id
  provisioner "local-exec" {
    command = "echo ${self.public_dns} >> /root/citadel_public_dns.txt"
  }
}
```

## 3\. Deploy

```plaintext
terraform plan
terraform apply
```

Thank you so much for taking your valuable time to read

I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner.

Hope you learned something new today :)

Signup to [KodeKloud](https://kodekloud.com/) for performing this challenge.

<iframe src="https://giphy.com/embed/l2YWFxG9GxXk8A7w4" class="giphy-embed" width="480" height="360"></iframe>

[via GIPHY](https://giphy.com/gifs/obama-barack-obama-potus-l2YWFxG9GxXk8A7w4)