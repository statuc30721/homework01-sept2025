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