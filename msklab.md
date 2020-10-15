# Produce and Consume messages from MSK topic using AWS SDK CLI
This lab helps you learn how to publish messages to a Kafka topic in MSK and consume them using AWS SDK using AWS Cloud9 CLI. Please complete the following pre-requisites in preperation to the lab,

## Pre-Requisite (Skip)
Create and setup a new MSK cluster using the twho cloud formation templates in the workshop link below. 
https://amazonmsk-labs.workshop.aws/en/msklambda/setup.html

## Connect to Kafka Client Machine using ssh and Cloud9
1. On your AWS console, search for "cloud9" and click on the service. Then click on Open IDE. It should take you to a landing page with MSKClient as seen in screenshot below. 

![screenshot](img/picture1.jpg)

2. Using cloud9, connect to Kafka client EC2 instance via ssh using steps below (Skip)
    a. use the ec2 keypair pem file you downloaded during pre-requisite and open it in a text editor.
    b. Select everything in the file (Ctrl+A) and copy.
    c. In the cloud9 console, type "vi keyfile.pem" and hit enter.
    d. Paste the contents you saved and save the file.
    e. chmod 600 keyfile.pem
    f. ssh -i keyfile.pem ec2-user@<Hostname_of_ec2_instance_seen_in_CF_stack_named_MSK_with_key_name_SSHKafkaClientEC2Instance>

Now, lets verify connectivity to your cluster and describe cluster. To run describe command, you will need the MSK cluster Arn which can be obtained using these steps. AWS Console --> MSK (Search msk under services)--> Clusters --> YourClusterName --> Details tab --> General (click the two little squares to copy)

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

1. First step to produce a message is to create a topic using the command below replacing the Zookeeper connection string.

*/home/ec2-user/kafka241/bin/kafka-topics.sh  --create --zookeeper ReplaceWithYourZookeeperConnectString --replication-factor 3 --partitions 1 --topic AWSKafkaTutorialTopic*

Zookeeper connectrion string can be found at, AWS Console --> MSK--> click on ClusterName --> Details Tab, Click View client information --> Scroll down for Zookeeper Connection (click the two squares to copy)

2. To publish messages using CLI, run the following command replacing the broker connection string.

*/home/ec2-user/kafka241/bin/kafka-console-producer.sh --broker-list ReplaceWithYourBootstrapBrokerStringTls --topic AWSKafkaTutorialTopic --property parse.key=true --property key.separator=":"*

Broker bootstrap string can be found at, AWS Console --> MSK--> click on ClusterName --> Details Tab, Click View client information --> Bootsrap servers --> Plaintext (click the two squares to copy)

Enter the following message payload in your command prompt either one line at a time or all together..
```
key1:the lazy
key2:fox jumped
key3:over the
key4:brown cow
key1:All
key2:streams
key3:lead
key4:to
key1:Kafka
key2:Go to
key3:Kafka
key4:summit
```
3. Now you have successfully published a message. Hit Ctrl+C to exit

## Consume messages from an MSK Topic
1. Run the command below to consume messages from the Kafka topic. Notice the end of the command that reads all messages from the beginning. 

*/home/ec2-user/kafka241/bin/kafka-console-consumer.sh --bootstrap-server ReplaceWithYourBootstrapBrokerStringTls--topic AWSKafkaTutorialTopic --from-beginning --property print.key=true --property key.separator=":"*

Broker bootstrap string can be found at, AWS Console --> MSK--> click on ClusterName --> Details Tab, Click View client information --> Bootsrap servers --> Plaintext (click the two squares to copy)

2. You should see the exact messages that you sent earlier and you will also notice the order of the messages preserved as you are only using 1 partition. Now, hit Ctrl+C to exit and it will tell the number of messages processed. 

3. It is a best practice for consumers to keep track of the message positions by partition that they processed which is called offset. Consumers, request reading messages from a specific offset to resume from where they left off. Lets assume you have read 10 messages earlier and need to read the rest. Use the command below to read the rest of the messages. 

*/home/ec2-user/kafka241/bin/kafka-console-consumer.sh --bootstrap-server ReplaceWithYourBootstrapBrokerStringTls--topic AWSKafkaTutorialTopic --property print.key=true --property key.separator=":" --partition 0 --offset 10*

4. You should see only 2 messages.

Congratulations!!! You have successfully completed this lab!!! YOu have learned how to create a topic, publish and consume messages from a Kafka Topic.
