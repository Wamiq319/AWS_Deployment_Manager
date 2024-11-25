#!/bin/bash

# Step 1: Create an EC2 Instance
# 1. Log in to AWS Management Console and launch an EC2 instance with Ubuntu.
# 2. Configure security groups: HTTP (port 80), HTTPS (port 443), and SSH (port 22).
# 3. Download the SSH key pair (your_key_pair.pem).

# Step 2: Test Flask App Locally
# 1. Navigate to your Flask app directory and test it locally:
cd /path/to/your/flask-app
python app.py
# Open http://127.0.0.1:5000/ in your browser to ensure the app is running correctly.

# 2. Clean Up (optional):
# Remove virtual environment folder and unnecessary dependencies if any:
rm -rf venv

# 3. Generate requirements.txt
pip freeze > requirements.txt

# Step 3: Organize Files for Transfer
# Create an 'AWS' folder in your Downloads directory and move your Flask app and .pem SSH key into it:
mkdir ~/Downloads/AWS
mv ~/path/to/your/flask-app ~/Downloads/AWS/
mv ~/path/to/your_key_pair.pem ~/Downloads/AWS/

# Step 4: Connect to EC2 via SSH
# Connect to your EC2 instance using SSH:
ssh -i ~/Downloads/AWS/your_key_pair.pem ubuntu@your_ec2_public_ip

# Update the server:
sudo apt update && sudo apt upgrade -y

# Exit the SSH session:
exit

# Step 5: Transfer Flask App to EC2 Instance
# Use SCP to copy your Flask app to the EC2 instance:
scp -i ~/Downloads/AWS/your_key_pair.pem -r ~/Downloads/AWS/flask-app ubuntu@your_ec2_public_ip:/home/ubuntu/

# Step 6: Set Up Flask App on EC2
# SSH back into your EC2 instance:
ssh -i ~/Downloads/AWS/your_key_pair.pem ubuntu@your_ec2_public_ip

# Navigate to the Flask app directory:
cd flask-app

# Create a virtual environment:
python3 -m venv venv

# Activate the virtual environment:
source venv/bin/activate

# Install dependencies from requirements.txt:
pip install -r requirements.txt

# Run the Flask app:
python app.py
