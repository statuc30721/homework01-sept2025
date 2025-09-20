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

    [ALTERNATIVES]

     One alternative is to add a second security group with a SSH rule.
     
     Template the running EC2 instance, modify the userdata input from the ec2script.txt with different information. For example have a picture hosted in a AWS S3 bucket or from a public or private accessible website (e.g. Github).
     
     For this project we will template the EC2 instance, load a revised ec2script and launch a new EC2 instance from the template.

    ## BAMC 1.1

    In the EC2 console select the running EC2 instance named My-HTTP-Server.    
    - Select under Actions, Image and templates, Create template from instance.
   
   ![template-menu](/graphics/aws-ec2-create-template-menu-screenshot.png)

    - Input a name for the template. In this example we will name the template HW01-BAMC-Template.

    - Launch template name: HW01-BAMC-Template.

    - Template version description: HW01-BAMC-Template.

    - Template Tags: Add creation date to help keep track of when the template was created.


    ![AWS-EC2-Create-Launch-Template](/graphics/aws-ec2-create-launch-template.png)


    In the Advanced section select user data perform the following steps:
    - Clear the current content in the user data field.
    - Copy the content from the modified ec2script file in BAMC-1.1/ec2script-1.1.txt.
    - Clear the User data section and input the content from the file BAMC-1.2/ec2script-1.2.txt.
        - Open the file and select "Copy Raw File" using the GitHub interface.
    - Save template.

    If everything worked then you should see a GREEN success message that you now have a template.

    

    Review the created template.
    - Select Launch templates from the left menu Instances > Launch Templates.       
    - You should see your newly created template.
    - Select the advanced tab and you should see your modified e2 script contents.

    Launch a new EC2 instance from the template.
    - Select actions > Launch instance from template.
    - Select under the Summary tab, Launch Instance.
    - You should see a GREEN successful EC2 launch.

    [NOTE] If you still have other EC2 instances running you will see your new EC2 instance running. Since it is running in the same VPC, your Security Group settings will control access to the EC2 instance.

    Verify changes to the web service running on the EC2 instance which has your modified index.html file.

    ![BMC1.1](/graphics/aws-ec2-bmc-1.1.png)

    ## BAMC 1.2
    
    For this version we will use the same procedures as above except we will modify the template.

    - Select EC2 > Launch Templates.
    - Select our existing Launch Template (e.g. HW01-BAMC-Template).
    - Select Actions, Launch Instance from template.
    - Open the Advanced details section at the bottom of the web page.
    - Clear the User data section and input the content from the file BAMC-1.2/ec2script-1.2.txt.
        - Open the file and select "Copy Raw File" using the GitHub interface.

        You should see your modified configuration for the index.html file.
    - Under Summary, select Launch Instance

    - Select the instance URL in the GREEN Successfully initiated launch of instance popup.

    - Verify your modification by selecting the Public DNS record and opening the URL in a new browser tab or window.

    ![BMC-1.2](/graphics/aws-ec2-bmc-1.2.png)

    [NOTE] You can and *should* use your own pictures to customize your test web page!

# Teardown Instructions

In any deployment on AWS we want to make sure we remove all objects we created. This is a good practice especially for proof of concept or demonstration projects like this one.

- Access the EC2 console and terminate any running instances.

- Select each running EC2 instance and select Instance State > Terminate Instances

![AWS Terminate EC2 Example](/graphics/aws-ec2-terminate-instance.png)

- Select the refresh icon to make sure all instances are terminated.

![AWS EC2 terminated](/graphics/aws-ec2-terminated-instances.png)

- Remove the Security Group we created for this demonstration.

- Network & Security > Security Groups
 - Select the security group we named SG-01 or whatever name you chose to use.

 [WARNING] DO NOT TOUCH the *default* security group of the VPC!!!

 ![AWS VPC Custom SG](/graphics/aws-vpc-custom-security-group.png)

- Refresh the window and you should see your original default VPC security group.

- Delete Launch Template
 - Instances > Launch Templates
 - Select the Launch Template ID that was created in previous steps. In this example that is HW01-BAMC-template.

 - Select Actions > Delete Template

 ![Delete-Template](/graphics/aws-ec2-template-delete.png)

 - In the Delete Launch Template menu you should read the note to understand that this is a irreversible action.

 - Input the confirmation word, and select Delete.

 ![delete template confirmation](/graphics/aws-ec2-template-delete-confirmation.png)

 If the deletion succeeded you should see a GREEN popup that states that the Deleet Launch Template Request succeeded. 

Return to your AWS EC2 Dashboard and refresh the web page.

In the Resources you should not see any Running Instances. You may see a number in Instances but those should match the instances you terminated.

As pictured below in the EC2 dashboard I have a quantity of 2 instances. Selecting the Instances shows that those match the last 2 instances that were terminated. You will notice in the screenshot that the dashboard shows no Instances in a *running* state.

![EC2 Dashboard](/graphics/ec2-dashboard-instance.png)


![EC2 Terminated Instances](/graphics/aws-ec2-terminated-instances.png)