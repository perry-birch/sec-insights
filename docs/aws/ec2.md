# Setup EC2 Instsance

## Go to the EC2 Dashboard

![EC2 Dashboard](./ec2/ec2_step01.png)

## Start the Launch Instance workflow

![Launch Instance](./ec2/ec2_step02.png)

## Name the Instance

![Name Instance](./ec2/ec2_step03.png)

## Select Operating System - Ubuntu

![Select Ubuntu](./ec2/ec2_step04.png)

## Large Instance Size

![Select t2.large](./ec2/ec2_step05.png)

## Configure Your Key Pair

![Create Key Pair](./ec2/ec2_step06.png)

![Define Key Pair](./ec2/ec2_step07.png)

![Select Key Pair](./ec2/ec2_step08.png)

## Increase Default Storage

![Set Disk Size](./ec2/ec2_step09.png)

## Find New Instance

![Find Instance](./ec2/ec2_step10.png)

## Connect with EC2 Instance Connect

![Find Instance](./ec2/ec2_step11.png)

## Connect with Terminal

![Find Instance](./ec2/ec2_step12.png)

## Connect the Terminal

- [~/Desktop/sec-insights] is the folder where the .pem file was downloaded
- Note the instructions (using the name you selected for your pem file):
```sh
chmod 400 "llama-sec-insights.pem"
```

![Find Instance](./ec2/ec2_step13.png)

## Next Steps

[Configure Ubuntu](./ubuntu.md)