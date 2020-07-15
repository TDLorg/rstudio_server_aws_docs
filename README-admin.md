# Setting up an RStudio Server on AWS

## Overview

These instructions are intended for CRUK FGC or TDL Bioinformaticians to aid in the setting up of an RStudio Server on AWS.

## Create an EC2 instance for RStudio

1. Select an RStudio Server AMI in the region of interest from a list of [pre-built images](https://www.louisaslett.com/RStudio_AMI/).
2. Make changes to the defaults to suit the required processor and memory specifications.
3. On the 'Configure Security Group' step add a new row for an HTTP 'Type' with the port set to 80 and the Source set to 'Anywhere' (Change SSH source to 'Anywhere' also).
4. Select a key-pair or create a new one.
5. Launch the EC2 instance and take note of the public IP address and Instance ID.

## Access via SSH

1. The key-pair 'pem' file is needed and must be readable only by root (`chmod 400 key.pem`):

```sh
ssh -i key.pem ubuntu@<ip-address>
```

## RStudio access

1. Enter the public IP address of the EC2 instance into your browser's address bar and press return.
2. The RStudio username is 'rstudio' and the password is the EC2 Instance ID.

## Create a new RStudio user

1. SSH into the machine as user 'ubuntu' (see above).
2. Create a new user:

```sh
sudo adduser <username>
```
3. In the new users home folder (`/home/<username>`), create an `R` subdirectory.
4. Provide the IP address and username and passwords defined in Step 1 above to the new user and point them towards the [user documentation](./README-user.md).
