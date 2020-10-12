# Produce and Consume messages from MSK topic using AWS SDK CLI
This lab helps you learn how to publish messages to a Kafka topic in MSK and consume them using AWS SDK using AWS Cloud9 CLI. Please complete the following pre-requisites in preperation to the lab,

## Pre-Req
Create and setup a new MSK cluster using the steps in workshop link below. 
https://amazonmsk-labs.workshop.aws/en/msklambda/setup.html

Lets verify connectivity to your cluster and describe cluster. To run describe command, you will need the MSK cluster Arn which can be obtained using these steps. AWS Console --> MSK (Search msk under services)--> Clusters --> YourClusterName --> Details tab --> General (click the two little squares to copy)

## Produce messages to MSK Topic
1. On your AWS console, search for "cloud9" and click on the service. It should take you to a landing page with MSKClient as seen in screenshot below. 

![screenshot](img/picture1.jpg)

2. 
