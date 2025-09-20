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

    - In Instance Type: select t3.micro. It may be a different Instance Type depending on when you are using this guide. It also may vary depending on what AWS region you are deploying an EC2 instance (e.g. Tokyo, Germany).

    - Create a new key pair. 
        - Name the SSH key pair the name of your EC2 instance.
            - Key pair name: My-HTTP-Server
            - Key pair type: RSA
            - Private key file format: .pem
        - Select Create key pair button.

        ![Create Key pair](/graphics/aws-ec2-create-key-pair.png)


        [NOTE] A PEM file with the name of your key will be downloaded to your management system that you are accessing the AWS EC2 dashboard. Please be sure to store the file in a safe place.

        - Set permission on the downloaded .pem file to 0400. This will set the owner to have Read permission, Groups and users will have no Read, Write or Execute permission to the file. For this type of file you only need Read permission.

    - Network Settings.
        - Leave Network, Subnet and auto-assign public IP in their default configuration.

        - In Firewall (security groups), select existing security group
            - Select the security group you created earlier. In this example we named it SG-01.

        ![network settings](/graphics/aws-ec2-network-settings.png)

    - Configure Storage: Leave this as is.

        ![configure storage default](/graphics/aws-ec2-configure-storage.png)

    - Advanced details.
        [CAUTION] There are a lot of settings here that can be modified. For this project we will *ONLY* leverage the user data feature.

        - In the user data field you will see the option to either choose a file or paste content into the web browser interface. In our project we will just paste the content from the ec2script.txt file into the space.

        - Open the ec2script.txt file and copy and paste the contents into the user data field.

        ![user-data-field](/graphics/aws-ec2-user-data-with-content.png)
    
    - Under the Summary field select Launch Instance. 

        ![EC2 summary](/graphics/aws-ec2-summary.png)

        If all of your settings were correct then you should see a green banner stating that the launch was successful.

        ![Launch successful](/graphics/aws-ec2-launch-successful.png)

    - Select the URL for the EC2 instance in the GREEN success popup in the web browser.

    You will see a lot of information about your EC2 instance including its Public and Private IPv4 addresses. Our objective is to make sure we can access the HTTP service application on the EC2 instance.

    - Select the Public DNS address which should be visible in the Instance summary. This IP address can change if you terminate and/or relaunch the EC2 instance.

    [NOTE] Recall that we are only using the HTTP protocol. So to confirm we have a working service we need to set the browser to use HTTP and *not* HTTPS.

    ![Webpage-example-deployment](/graphics/example-webpage.png)

    [NOTE] You may notice that you can't access the EC2 instance via the AWS EC2 Instance connect or via Secure Shell even though you added a SSH key to the EC2 instance.

    To be able to access the EC2 instance via Secure Shell you will need to modify the existing Security Group and add allow SSH inbound. 

    [Alternatives]
        1. One alternative is to add a second security group with a SSH rule.

        2. Template the running EC2 instance, modify the userdata input from the ec2script.txt with different information. For example have a picture hosted in a AWS S3 bucket or from a public or private accessible website (e.g. Github).

    For this project we will template the EC2 instance, load a revised ec2script and launch a new EC2 instance from the template.

    -BAMC 1.1
     
    1. In the EC2 console select the running EC2 instance named My-HTTP-Server.
    2. Select under Actions, Image and templates, Create template from instance.
        - Input a name for the template. In this example we will name the template HW01-BAMC-Template.

        - Launch template name: HW01-BAMC-Template

        - Template version description: HW01-BAMC-Template

        - Template Tags: Add creation date to help keep track of when the template was created.
    3. In the Advanced section select user data and input the modified ec2script file under the BAMC-1.1 folder named ec2script-1.1.txt.

     





