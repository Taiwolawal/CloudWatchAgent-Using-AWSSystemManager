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
Click on create parameter when done.

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

## Step 5: Check Metrics

<img width="1377" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/8c72ff8c-669a-4eb1-8e54-0c75c5b6a55f">

<img width="1166" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/daab4a53-b71a-4d18-92f9-7777ea62640b">

<img width="1157" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/2e6663bd-f0c4-4832-819a-8c72bf8b56e1">

## Step 6: Display Metrics on Grafana

- Setup an instance (ubuntu) and install grafana on it and ensure you set the security group by opening port 3000 for grafana

<img width="1357" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/704e46d2-484c-4c04-b28d-d66256755965">

- Install grafana

```
sudo apt update
sudo apt-get install -y gnupg2 curl software-properties-common
sudo curl https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt update
sudo apt -y install grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
systemctl status grafana-server
```

<img width="1418" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/407b2f24-1e50-4ca1-bd19-7296b9f50e41">

- Connect to grafana using the public ip address with port 3000 ```<public-ip-address>:3000```.

<img width="1029" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/6680ada3-697d-46d1-945e-2637889a1304">

- Access the grafana UI using admin for both username and password and change your password to your choice.

<img width="1439" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/b04ab93d-8ad1-4b09-9064-2a2126739dc1">

- Configure data source by adding your data source and choose cloudwatch
  
<img width="1434" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/671bfb6e-8d93-46d6-addc-b6667f12bbc1">

<img width="1432" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/03f8e095-126c-440b-992f-ce061c42804b">

- Integrating Grafana with AWS
  
1. Create IAM Policy

<img width="1362" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/6cdd2316-312c-466c-a5da-4fe40851e6e6">

<img width="1389" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/503f2d29-c0e8-424f-960f-dae01a0479da">

2. Create IAM Role: Select AWS Services and EC2

<img width="1387" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/227f69b3-b487-4f04-9dbe-b0d1594013b9">

3. Add Permission by adding the policy created earlier and create the role

<img width="1389" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/20fb2d57-8e54-448e-b940-2efdeef8087c">

<img width="1390" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/ca2bf499-4956-4252-934e-deebf34856c0">

<img width="1410" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/15990c80-2bd4-4b25-b9c6-5cce0479be9b">


4. Establish IAM user who has access to the IAM role and IAM policy you just created.

<img width="1382" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/0ecc967b-09ca-4821-b0be-2237d1425568">

5. Attach the policy to the user

<img width="1384" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/670864a2-4b44-4013-b5a4-74f38b4fdaa5">

<img width="1377" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/861e386d-2b22-4c8c-a004-fc0821b62cb3">

6. Create access key credentials for the user

<img width="1199" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/5c0c4d99-2ad6-4376-9aa3-decc6b0401dd">

<img width="1212" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/17b4049a-b17f-493b-a5f4-7296156f4fd3">

<img width="1169" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/90cdb915-a7b9-46e5-8130-e0340a2be248">

7. Create credentials file in the grafana server

<img width="569" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/08417f04-b6a5-46c7-a4fd-1dc1dce6b5ad">

<img width="581" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/b0b079a0-6c8f-4ffb-a5c0-af1e64180a12">


6. Attach the role to grafana instance

<img width="1407" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/d78b4360-88e8-4d6d-a940-6c63230fca0f">

<img width="740" alt="image" src="https://github.com/Taiwolawal/CloudWatchAgent-Using-AWSSystemManager/assets/50557587/9a692cb3-c171-45d0-b43e-2915646699d2">

7. Connect grafana to Cloudwatch: I got some errors, trying to comm
























