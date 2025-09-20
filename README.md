# Amazon Web Service (AWS) EC2 Deployment

Deploy a basic EC2 instance in an AWS region.

1. Access AWS console via web browser application.

2. Set AWS region to us-east-2. Move to the EC2 dashboard.

![EC2 Dashboard](/graphics/aws-ec2-dashboard.png)

3. There should be a default Virtual Private Cloud. You can use the default or create a new VPC. A good practice is to create a new VPC for projects and leave the default VPC alone.

4. Create a Security Group (SG) with only a HTTP inbound rule.

- Under Network & Security > Security Groups
    - Select Create Security Group
    - Input a Name and Description for the SG
    - Verify the VPC is the default that the SG
    - In Inbound Rules add a rule of 
        - Type: HTTP
        - Protocol: TCP
        - Port Range: 80
        - Source: anywhere-IPV4 # Note this not a best practice. 
        - Description 
    - Outbound Rules will *NOT* be modified. Leave this setting as-is.
    - Tags: Add tags to support being able to find or have an idea what or when this was created. 

    ![SecurityGroup](/graphics/aws-security-group-01.png)

    - Select Create Security Group 
    - Verify the newly created SG has the settings you require.

    ![Completed SG](/graphics/completed-security-group.png)

5. Obtain a startup script for your EC2 instance.

    - Create or use a provided startup script that will be used by the EC2 instance when it is initially deployed in the AWS VPC.

    - For this instance it is a file named ec2script.txt. Under normal operations it would have a filename extension as ".sh". In this deployment we leave it as a text file. 

6. EC2 Deployment

    - In the EC2 dashboard menu, select Instances
    - Select Launch Instances
    - Under name and Tags input a name. For example My-HTTP-Server
    - Select Amazon Linux Machine Image (AMI) with AWS logo. You have other types but for this project we will stay with the basic Linux AMI.
    - Note the username for the image. The default user is ec2-user.
    - In Instance Type: select t3.micro. It may be a different Instance Type depending on when you are using this guide. It also may vary depending on what AWS region you are deploying an EC2 instance (e.g. Tokyo, Germany)

    