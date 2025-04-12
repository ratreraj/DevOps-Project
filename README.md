# DevOps-Project


# 1.	Create an Instance in AWS manually 
Below is a detailed, step-by-step guide to launching an AWS EC2 instance with a t2.micro instance type, creating a new key pair, and configuring the security group to allow SSH access:

---
  
  ## 1. Sign in to the AWS Management Console
  - **Log In:** Open your web browser and go to the [AWS Management Console](https://aws.amazon.com/console/). Log in with your AWS credentials.
  - **Navigate to EC2:** Once logged in, type “EC2” in the service search bar or click on **Services** > **EC2** to open the Amazon EC2 Dashboard.
  
  ---
  
  ## 2. Start the Instance Launch Wizard
  - **Launch Instance:** In the EC2 Dashboard, click the **Launch Instance** button to begin configuring your new instance.
  
  ---
  
  ## 3. Choose an Amazon Machine Image (AMI)
  - **Select AMI:** Choose an AMI that suits your needs. For a typical Linux instance, you might select an **Amazon Linux 2 AMI** or an Ubuntu image. Make sure that the AMI is compatible with the t2.micro instance type.
  But I am choosing Ubuntu - ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-20250305
  ---
  
  ## 4. Choose the Instance Type
  - **Select t2.micro:** On the **Choose an Instance Type** page, scroll through the list and select **t2.micro** (this instance type is part of the free tier, if eligible). Then click **Next: Configure Instance Details**.
  
  ---
  
  ## 5. Configure Instance Details
  - **Basic Settings:** You can typically use the default settings for a basic instance setup. Some common options include:
    - Number of instances: **1**
    - Network: Select your default VPC (or a specific VPC as needed)
    - Subnet: Choose a subnet that fits your region or availability zone
  - **Advanced Options:** For beginners, the default configurations are usually sufficient. Click **Next: Add Storage**.
  
  ---
  
  ## 6. Add Storage
  - **Configure Storage:** Verify that the default storage (usually 8 GB for Amazon Linux) meets your needs, or adjust it as necessary. Once satisfied, click **Next: Add Tags**.
  
  ---
  
  ## 7. Add Tags
  - **Tagging the Instance:** Tags are key-value pairs that help you manage and organize your resources. For example, you can add:
    - **Key:** Name  
      **Value:** MyEC2Instance
  - Click **Next: Configure Security Group** when finished.
  
  ---
  
  ## 8. Configure Security Group (Open SSH Port)
  - **Create New Security Group:** When prompted, choose to create a new security group.
  - **Add Rule for SSH:**
    - **Type:** SSH
    - **Protocol:** TCP  
    - **Port Range:** 22  
    - **Source:** 
      - Option 1: For maximum accessibility (less secure), select **Anywhere** (0.0.0.0/0).  
      - Option 2: For a more secure setup, specify a custom IP or IP range (e.g., your home or office IP address).
  - **Save:** After adding the rule, click **Review and Launch**.
  
  ---
  
  ## 9. Review and Launch
  - **Review Configuration:** Carefully review all of your configuration settings – confirm the instance type (t2.micro), your AMI, storage settings, tags, and the security group rule you created.
  - **Launch Instance:** Click **Launch** when ready.
  
  ---
  
  ## 10. Create a New Key Pair
  - **Prompt for Key Pair:** After clicking Launch, a prompt will appear asking you to select a key pair.
    - **Select Create a New Key Pair:**
      - **Name Your Key Pair:** Give your key pair a descriptive name (e.g., `MyEC2KeyPair`).
      - **Download the Key Pair:** Click **Download Key Pair**. This will download a `.pem` file to your computer. **Keep this file secure!** You will need it later to SSH into your instance, and AWS does not store a copy.
    - **Acknowledge:** Check the box to acknowledge that you have access to the selected key pair file.
    - **Launch:** Click **Launch Instances** to complete the process.
  
  ---
  
  ## 11. Verify and Connect to Your Instance
  - **Monitor Instance State:** Return to the EC2 Dashboard where you can see your new instance. Wait until the instance state shows as **running**.
  - **SSH Connection:** To connect:
    1. Open your terminal.
    2. Change the permission of your downloaded key pair file:
       ```bash
       chmod 400 MyEC2KeyPair.pem
       ```
    3. Connect using the SSH command:
       ```bash
       ssh -i "MyEC2KeyPair.pem" ec2-user@<instance-public-dns>
       ```
       - Replace `<instance-public-dns>` with your instance’s public DNS, which you can find in the instance details.
    4. If you’re using Ubuntu, the default username is usually `ubuntu` instead of `ec2-user`.
  
  ---
  
  ## Additional Tips
  - **Secure Your Key Pair:** Never expose your `.pem` file publicly or check it into version control.
  - **Restrict SSH Access:** Using a restricted source IP for the SSH rule in your security group enhances security.
  - **Instance Lifecycle Management:** When you finish using your instance, remember to stop or terminate it to avoid ongoing charges.



# 2. Install Terraform & Ansible on ec2 Instance

  - **Create a sh file for terrfrom installtion**  
      ```bash
      sudo nano terrfrom.sh
      ```
    - Paste all code on sh file
      ```bash
      sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

      wget -O- https://apt.releases.hashicorp.com/gpg | \
      gpg --dearmor | \
      sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null      
      
      gpg --no-default-keyring \
      --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
      --fingerprint      
      
      echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
      https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
      sudo tee /etc/apt/sources.list.d/hashicorp.list      
      
      sudo apt update      
      
      sudo apt-get install terraform -y
      ```
    - Press s&x to save and exit
    - Run chmod +x command to provide access to file
      ```bash
      chmod +x terrfrom.sh
      ```
    - excute sh file for installtion
      ```bash
      ./terraform.sh
      ```
- **Crate sh file for ansible installtion**
    ```bash
      sudo nano terrfrom.sh
      ```
 - Paste all code on sh file
      ```bash
       sudo apt update
       sudo apt install software-properties-common
       sudo add-apt-repository --yes --update ppa:ansible/ansible
       sudo apt install ansible
      ```
    
    
