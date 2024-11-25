## Prerequisites

1. **AWS Account**: Make sure you have an active AWS account.
2. **AWS EC2 Instance**: You need to create an EC2 instance (Ubuntu) with SSH, HTTP, and HTTPS access.
3. **Flask App**: Ensure your Flask app is working well locally before proceeding.

## Step 1: Create an EC2 Instance

1. Log in to your [AWS Management Console](https://aws.amazon.com/console/).
2. Go to the **EC2 Dashboard** and click on **Launch Instance**.
3. Choose **Ubuntu Server** as your OS.
4. Select an instance type (e.g., t2.micro for free tier).
5. Configure security groups:
   - Allow **HTTP** (port 80)
   - Allow **HTTPS** (port 443)
   - Allow **SSH** (port 22)
6. Launch the instance and download the SSH key pair (.pem file).

## Step 2: Test Flask App Locally

1. Make sure your Flask app is running locally without errors.
   - Navigate to the folder containing your Flask app and test it by running:
     
bash
     python app.py

   - Open http://127.0.0.1:5000/ in your browser to verify the app works.

2. **Clean Up**: Delete unnecessary modules and any virtual environments (optional but recommended):
   - Remove the virtual environment folder and unused dependencies.

3. **Dependencies**: Create a requirements.txt file with all the dependencies needed to run the app:
   
bash
   pip freeze > requirements.txt
# Step 3: Organize Files for Transfer
# Create a folder named 'AWS' in your Downloads directory (for organization)

# Move the .pem SSH key and your Flask app folder (e.g., flask-app) into the AWS folder


# Step 4: Connect to EC2 via SSH
# Open a terminal and connect to your EC2 instance
ssh -i ~/Downloads/AWS/your_key_pair.pem ubuntu@your_ec2_public_ip

# Once connected, update the server
sudo apt update && sudo apt upgrade -y

# Exit the SSH session
exit

# Step 5: Transfer Flask App to EC2 Instance
# Use the scp command to copy your Flask app files to the EC2 instance

scp -i your_key_pair.pem -r your_local_directory_path user@your_ec2_public_ip:/path/on/ec2/instance
e.g scp -i flask-app.pem -r aws ubuntu@ec2-3-7-185-177.ap-south.compute.amazonaws.com:/ubuntu/home/Flask-app

# Step 6: Set Up Flask App on EC2
# SSH back into your EC2 instance
ssh -i ~/Downloads/AWS/your_key_pair.pem ubuntu@your_ec2_public_ip

# ls and check directory in your ubuntu you will see flask-app cd in init 
create virtual enviroment by 
python venv .venv create
run virtual enviroment
 install flask in it 
 install all the dependencies in it curul the app 
 using curl local host
 using pip install requiremnt.txt
 run python app using 
 python app.py 
 
 

# Install the necessary dependencies
cd flask-app
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Run the Flask app
python app.py
