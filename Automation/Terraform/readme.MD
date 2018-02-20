**Use Terraform to create a Linux VM in Azure from a Packer Custom Image with NGINX installed **

Terraform is a tool (templating language) for building, changing, and versioning infrastructure safely and efficiently. Using Terraform, you can automate the tasks of building, changing and de-provisioning(destroy)the infrastructure

#Steps

## Install Terraform and configure it to access Azure
Installation instructions can be found at the following URL

https://www.terraform.io/intro/getting-started/install.html

## Create SSH Key

Generate an SSH key pair. You can find a detailed instructions on how to create an SSH key pair at the URL below. 

https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-ssh-keys-detailed

## Install Packer

Installation instructions can be found at the following URL

https://www.packer.io/docs/install/index.html

## Use Packer to Create an Ubuntu image with NGINX installed

The article below details how to use Packer to define and build a custom image in Azure

https://docs.microsoft.com/en-us/azure/virtual-machines/linux/build-image-with-packer



