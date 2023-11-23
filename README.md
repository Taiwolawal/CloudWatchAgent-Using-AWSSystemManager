# CloudWatchAgent-Using-AWSSystemManager

Installing AWS CloudWatch Agent with AWS Systems Manager

The project demonstrates how to automate the installation of CloudWatch Agent using AWS System Manager and monitoring and alerting via Grafana

## Step 1: Create an IAM Role and attach policies "CloudWatchAgentServerPolicy" and "AmazonSSMManagedInstanceCore"

<img width="1153" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/e27982a0-1590-4a93-9a10-19d5d20068d4">

Attach the role created "CloudWatchAgentServerRole" to the EC2 instances, you are working with and confirm the role already attached

<img width="1402" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/18e95733-2c27-40e9-93b0-5782edcf4790">

<img width="819" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/2d92c406-894b-4079-82bf-7324b8667627">

<img width="776" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/7a09258b-9941-4260-b10c-2ecf083a32bf">

## Step 2: Download the CloudWatch Agent package on the target EC2 Instances

- Go to AWS Systems Manager --> Run Command

<img width="1238" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/5895b519-eb4e-4076-acf1-fb63ca659701">

<img width="1170" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/58099ff1-9b95-4f5d-9edf-2546c0e8fda2">

- Command document: Search for ```AWS-ConfigureAWSPackage```

<img width="1166" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/8782d501-b970-4b68-bb27-905893c7a836">

- Command Parameter: Update the Name and Version section with ```AmazonCloudWatchAgent``` and ```latest``` respectively.

<img width="1119" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/4da9aba2-0483-4113-bda0-064a92536f31">

Kindly take note that the name section can't be any name, the names required in the field are highlighted in the screenshot below. We are only interested in AmazonCloudWatchAgent

<img width="629" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/6523c175-c0ed-4699-8d72-e0d466774185">

- Target selection: Select the instances you want to download CloudwatchAgent on and hit Run at the end of the page

<img width="1117" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/878efad9-6b04-4a38-a494-7a6c0f88edc4">

<img width="1432" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/f09e0583-63ec-496d-8f48-289454f0dc72">

## Step 3: Save Agent Configuration in Parameter store

We will save the agent configuration file in AWS System Manager --> Parameter Store

<img width="1401" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/463c499a-a88b-4be9-856e-ff5f2070cf03">

Give a name for the Parameter ```AmazonCloudWatch-Metrics``` ,Tier ```Standard``` , type ```String```

<img width="878" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/937b8a24-9702-49b5-bcb0-572055f1f41a">

The configuration we are using is to show Disk and Memory metrics to CloudWatch. The value section will have this:

```
{
   "agent": {
      "metrics_collection_interval": 60,
      "run_as_user": "root"
   },
   "metrics": {
      "aggregation_dimensions": [
         [
            "InstanceId"
         ]
      ],
      "append_dimensions": {
         "ImageId": "${aws:ImageId}",
         "InstanceId": "${aws:InstanceId}",
         "InstanceType": "${aws:InstanceType}"
      },
      "metrics_collected": {
         "disk": {
            "measurement": [
               "used_percent"
            ],
            "metrics_collection_interval": 60,
            "resources": [
               "/"
            ]
         },
         "mem": {
            "measurement": [
               "mem_used_percent"
            ],
            "metrics_collection_interval": 60
         }
      }
   }
}
```
Click on Create parameter when done.

<img width="1118" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/10bb28e6-498c-48df-a82e-b45deada52a3">

![image](https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/8f59d098-42c1-4bb2-96b4-9339e007929f)

## Step 4: Install CloudWatch Agent package on the target EC2 instances

- Click on the Run command, on the command document search for ```AmazonCloudWatch-ManageAgent```

<img width="1165" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/3448de3e-4026-4e29-8f20-2bdc70fad2ff">

- Command Parameters: Ensure you enter the name of the parameter you created earlier ```AmazonCloudWatch-Metrics``` in the optional configuration section and ensure the optional configuration source is left as ```ssm```

<img width="1402" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/1cdc8366-0fce-4994-9b31-a9f25bfef8c3">

- Target selection: Select the instances you want to install the CloudWatch Agent on and click run at the end of the page

<img width="1118" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/182f0852-338c-4398-8346-b9ae45dd6919">

<img width="1180" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/8f145de9-ed1e-4426-998d-f4e1a207dcb5">














