
 Step 1: Create an EC2 Instance
 1. Log in to AWS Management Console and launch an EC2 instance with Ubuntu.
 2. Configure security groups: HTTP (port 80), HTTPS (port 443), and SSH (port 22).
 3. Download the SSH key pair (your_key_pair.pem).

 Step 2: Test Flask App Locally
1. Navigate to your Flask app directory and test it locally:
2. Clean Up (optional):
3Remove virtual environment folder and unnecessary dependencies if any:
4. Generate requirements.txt
```bash
pip freeze > requirements.txt
```


Step 3: Connect to EC2 via SSH
Connect to your EC2 instance using SSH:
```bash
ssh -i ~/Downloads/AWS/your_key_pair.pem ubuntu@your_ec2_public_ip
```
Update the server:
```bash
sudo apt update && sudo apt upgrade -y
```
Exit the SSH session:
exit

Step 4: Transfer Flask App to EC2 Instance
1Create an 'AWS' folder in your Downloads directory and move your Flask app and .pem SSH key into it:
2Use SCP to copy your Flask app to the EC2 instance:
```bash
scp -i ~/Downloads/AWS/your_key_pair.pem -r ~/Downloads/AWS/flask-app ubuntu@your_ec2_public_ip:/home/ubuntu/
e.g scp -i flask-app.pem -r aws ubuntu@ec2-3-7-185-177.ap-south.compute.amazonaws.com:/ubuntu/home/Flask-app
```
Step 6: Set Up Flask App on EC2
1 SSH back into your EC2 instance:
```bash
ssh -i ~/Downloads/AWS/your_key_pair.pem ubuntu@your_ec2_public_ip
```
2 Navigate to the Flask app directory:
```bash
cd flask-app
```
 Create a virtual environment:
```bash
python3 -m venv venv
```
 Activate the virtual environment:
```bash
source venv/bin/activate
```

Install dependencies from requirements.txt:
pip install -r requirements.txt

Run  and test the Flask app:
```bash
python app.py
curl http://your_ec2_public_ip:5000
e.g http://2-3-7-185-177:5000
```
Press ctrl+ c to quit the app 
#MAKE SURE YOU ARE IN VIRTUAL ENVIROMENT YOU CAN ALSO INSTALL THESE IN UBUNTU MACHINE
Step7:Run Gunicorn WSGI server to serve the Flask Application
When you “run” flask, you are actually running Werkzeug’s development WSGI server, which forward requests from a web server.
Since Werkzeug is only for development, we have to use Gunicorn, which is a production-ready WSGI server, to serve our application.

Install Gunicorn using the below command:
```bash
pip install gunicorn
```
Run Gunicorn:
```bash
gunicorn -b 0.0.0.0:8000 app:app 
```
Gunicorn is running (Ctrl + C to exit gunicorn)!

Use systemd to manage Gunicorn
Systemd is a boot manager for Linux. We are using it to restart gunicorn if the EC2 restarts or reboots for some reason.
We create a <projectname>.service file in the /etc/systemd/system folder, and specify what would happen to gunicorn when the system reboots.
We will be adding 3 parts to systemd Unit file — Unit, Service, Install

Unit — This section is for description about the project and some dependencies
Service — To specify user/group we want to run this service after. Also some information about the executables and the commands.
Install — tells systemd at which moment during boot process this service should start.
With that said, create an unit file in the /etc/systemd/system directory
	
```bash
sudo nano /etc/systemd/system/FLASK-APP.service
```
Then add this into the file.
```bash
[Unit]
Description=Gunicorn instance for a simple flask app
After=network.target
[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/Flask-app(in hwhich you have app.py and other files )
ExecStart=/home/ubuntu/Flask-app/venv/bin/gunicorn -b localhost:8000 app:app
Restart=always
[Install]
WantedBy=multi-user.target
```
Then enable the service:
```bash
sudo systemctl daemon-reload
sudo systemctl start FLASK-APP
sudo systemctl enable FLASK-APP
```
Check if the app is running with 
```bash
curl localhost:8000
```
#step8:Run Nginx Webserver to accept and route request to Gunicorn
Finally, we set up Nginx as a reverse-proxy to accept the requests from the user and route it to gunicorn.

Install Nginx 
```bash
sudo apt-get nginx
```
Start the Nginx service and go to the Public IP address of your EC2 on the browser to see the default nginx landing page
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```
Edit the default file in the sites-available folder.
```bash
sudo nano /etc/nginx/sites-available/default
```
Add the following code at the top of the file (below the default comments)
```bash
upstream flask-app-server {
    server 127.0.0.1:8000;
}
```
Add a proxy_pass to flaskhelloworld atlocation /
```bash
location / {
    proxy_pass http://flask-app-server;(Same Name as name set for upstream)
}
```
Restart Nginx 
```bash
sudo systemctl restart nginx
```
Tada! 🎉 Our application is up! 🚀


