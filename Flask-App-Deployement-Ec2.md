# Deploy Flask App on AWS EC2 with Nginx and Gunicorn

```bash
# Install Required Packages
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-pip python3-venv -y
sudo apt install nginx -y

# Set Up Flask Application
git clone https://github.com/your-repo/flask-app.git
cd flask-app
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Install Gunicorn
pip install gunicorn
gunicorn --bind 0.0.0.0:8000 wsgi:app

# Configure Nginx
sudo nano /etc/nginx/sites-available/flask-app
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /path-to-your-app/static/;
    }
}

[Unit]
Description=Gunicorn instance to serve Flask App
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/path-to-your-app
Environment="PATH=/path-to-your-app/venv/bin"
ExecStart=/path-to-your-app/venv/bin/gunicorn --workers 3 --bind unix:flask-app.sock -m 007 wsgi:app

[Install]
WantedBy=multi-user.target
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your-domain.com
# Check Gunicorn logs
sudo journalctl -u flask-app

# Check Nginx logs
sudo tail -f /var/log/nginx/error.log


Just copy-paste the above content into your `README.md` file. You can replace the placeholders (`your-repo`, `your-domain.com`, `/path-to-your-app`, etc.) with actual values as needed.
