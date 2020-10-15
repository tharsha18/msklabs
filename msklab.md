# Produce and Consume messages from MSK topic using AWS SDK CLI
This lab helps you learn how to publish messages to a Kafka topic in MSK and consume them using AWS SDK using AWS Cloud9 CLI. Please complete the following pre-requisites in preperation to the lab,

## Pre-Req
Create and setup a new MSK cluster using the twho cloud formation templates in the workshop link below. 
https://amazonmsk-labs.workshop.aws/en/msklambda/setup.html

## Connect to Kafka Client Machine using ssh and Cloud9
1. On your AWS console, search for "cloud9" and click on the service. Then click on Open IDE. It should take you to a landing page with MSKClient as seen in screenshot below. 

![screenshot](img/picture1.jpg)


Lets verify connectivity to your cluster and describe cluster. To run describe command, you will need the MSK cluster Arn which can be obtained using these steps. AWS Console --> MSK (Search msk under services)--> Clusters --> YourClusterName --> Details tab --> General (click the two little squares to copy)

*aws kafka describe-cluster --region us-east-1 --cluster-arn "YourClusterArn"*

Output of the above command should look similar to below. Take a few minutes ro review it. 

`{
    "ClusterInfo": {
        "BrokerNodeGroupInfo": {
            "BrokerAZDistribution": "DEFAULT",
            "ClientSubnets": [
                "subnet-0ded0d3ee0ca01808",
                "subnet-0d92bffb39d24a289",
                "subnet-0c3818153bf4074bd"
            ],
            "InstanceType": "kafka.m5.large",
            "SecurityGroups": [
                "sg-048707eebd805819f",
                "sg-0d8bf192dd6579b58"
            ],
            "StorageInfo": {
                "EbsStorageInfo": {
                    "VolumeSize": 100
                }
            }
            ..
            ..
            ..`
            

## Produce messages to MSK Topic

2. 
