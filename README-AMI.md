# Instructions for setting up an RStudio Server AMI for the *latest* R and RStudio versions

## Overview

These instructions are for CRUK-TDL and FGC Bioinformaticians to aid in the creation of an Amazon Machine Image (AMI) for an RStudio Server built using the latest versions of R and RStudio.

## Set up an EC2 instance with a base RStudio Server image

1. Set up an RStudio Server (based on R version 3.6 and RStudio Server 1.2) EC2 instance using a [pre-built image](https://www.louisaslett.com/RStudio_AMI/) in the region of interest (make sure you are logged into the AWS account you want the EC2 instance in).
2. Make changes to the defaults to suit the required processor and memory specifications.
3. On the 'Configure Security Group' step add a new row for an HTTP 'Type' with the port set to 80 and the Source set to 'Anywhere' (Change SSH source to 'Anywhere' also).
4. Select a key-pair or create a new one.
5. Launch the EC2 instance and take note of the public IP address and Instance ID.


## Install dependencies for updating R and RStudio

1. The key-pair 'pem' file is needed and must be readable only by root (`chmod 400 key.pem`).
2. Log into the EC2 instance using your 'pem' file:
```
ssh -i key.pem ubuntu@<ip-address>
```
3. Install dependencies for upgrading R and RStudio:
```
sudo apt-get install libpcre2-dev libgtk2.0-dev xvfb libcairo2-dev xorg openbox
```

## Update `R`

1. Start `R` in the command line:
```
sudo R
```
2. Install `R` package `devtools`:
```
install.packages("devtools")
```
3. Install a specific version of the `XML` package:
```
devtools::install_version("XML", version = "3.98-1.5")
```
3. Install R package `ropenblas`:
```
devtools::install_github(repo = "prdm0/ropenblas", force = TRUE)
```
4. Compile and install the latest version of `R`:
```
ropenblas::rcompiler()
```
To install a specific version of `R`, use the `x` argument, e.g. `rcompiler(x = "3.6.2")`.

## Update RStudio Server

1. Download the latest RStudio Server Debian package, e.g.:
```
wget https://download2.rstudio.org/server/xenial/amd64/rstudio-server-1.3.1056-amd64.deb
```
2. Install RStudio Server, e.g.:
```
sudo gdebi rstudio-server-1.3.1056-amd64.deb
```

## Configure `rstudio` user for local `R` package installs

1. Create an `.Rprofile` file in `/home/rstudio` containing the following line of `R` code:
```
.libPaths("/home/rstudio/R")
```

## Create an AMI



