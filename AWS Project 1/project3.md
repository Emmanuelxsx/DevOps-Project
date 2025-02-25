## creating two separate stacks for Auto Scaling and Load Balancing and then attach the Autoscaling group to the Load Balancer using a Nested Stack.

## Architecture Diagram:
![alt text](<images/newproject3 im/ybIF76TDQp6MdpJCsxu-Sg_7f7269788cae4b2ab5318d4950a1c2a1_image.png>)

![alt text](<images/newproject3 im/Capture.PNG>)

Tasks Details:

1. Sign in to AWS Management Console.

2. Create Subnets using the VPC_Template cloud formation stack.

3. Create Subnets using the VPC_II_Template cloud formation stack.

4. Deep dive into the  VPC_Template and VPC_II_Template.

2. Task 2: Creating Subnets using the VPC Template CloudFormation stack

* Search for S3 by clicking on 'Services' in the top menu, then click on S3 .
![alt text](<images/newproject3 im/creating S3 bucket.PNG>)

* upload the vpc template file
![alt text](<images/newproject3 im/uploading1.PNG>)
![alt text](<images/newproject3 im/uploading2.PNG>)


* Open that bucket and click on the object named VPC_template.json.

* Next, copy the Object URL to the clipboard for use in the CloudFormation template.
![alt text](<images/newproject3 im/copying url.PNG>)

* Navigate to CloudFormation by clicking on 'Services'  in the top menu, then click on CloudFormation.

* Then click on 'Create stack' and select With new resources(standard).

![alt text](<images/newproject3 im/creating stack1.PNG>)


