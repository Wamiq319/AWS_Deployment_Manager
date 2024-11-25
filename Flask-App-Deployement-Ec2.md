# Flask App Deployment on AWS EC2

This tutorial guides you through the process of deploying a Flask application on an AWS EC2 instance. 

## Prerequisites

1. **AWS Account**: Make sure you have an active AWS account.
2. **AWS EC2 Instance**: You need to create an EC2 instance (Ubuntu) with SSH, HTTP, and HTTPS access.
3. **Flask App**: Ensure your Flask app is working well locally before proceeding.

## Step 1: Create an EC2 Instance

1. Log in to your [AWS Management Console](https://aws.amazon.com/console/).
2. Go to the **EC2 Dashboard** and click on **Launch Instance**.
3. Choose **Ubuntu Server** as your OS.
4. Select an instance type (e.g., `t2.micro` for free tier).
5. Configure security groups:
   - Allow **HTTP** (port 80)
   - Allow **HTTPS** (port 443)
   - Allow **SSH** (port 22)
6. Launch the instance and download the SSH key pair (`.pem` file).

## Step 2: Test Flask App Locally

1. Make sure your Flask app is running locally without errors.
   - Navigate to the folder containing your Flask app and test it by running:
     ```bash
     python app.py
     ```
   - Open `http://127.0.0.1:5000/` in your browser to verify the app works.

2. **Clean Up**: Delete unnecessary modules and any virtual environments (optional but recommended):
   - Remove the virtual environment folder and unused dependencies.

3. **Dependencies**: Create a `requirements.txt` file with all the dependencies needed to run the app:
   ```bash
   pip freeze > requirements.txt
4 Create a Aws folder in dowload (just for oragnization of the process) place the .pem ssh key and also the the folder with all the app files suppose app name is flask-app.Now ssh in you instance using 
   ```bash
  ssh-dhhd
5 upadte once sh you will see some thhing like this
 [img src="media/ssh-image.jpg"]
6 update the server with command
''bash
7  now close ssh connection using ctrl+D
8 now move all the files to server using
""ssh
 scp -i your_key_pair.pem -r your_local_directory_path user@your_ec2_public_ip:/path/on/ec2/instance
e.g
