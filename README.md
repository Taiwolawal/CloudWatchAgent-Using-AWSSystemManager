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









