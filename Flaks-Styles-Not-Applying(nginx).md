# README: Troubleshooting Nginx Static File Access Issues for Flask App

## Problem Overview

While hosting a Flask app on AWS EC2 using Gunicorn and Nginx, you encountered two main issues:

1. **Static Files Not Loading:** The stylesheets and other static files (e.g., `tailwind.css`) were not loading properly.
2. **403 Forbidden Errors:** When trying to access static files (e.g., `http://your-server-ip/static/css/tailwind.css`), Nginx was returning a `403 Forbidden` error.

### Key Findings

- The Flask app was tested with Gunicorn directly (on port 8000) and worked fine.
- The Nginx configuration seemed correct, but static files were not accessible, and a `403 Forbidden` error appeared.
- The static files' permissions and ownership appeared correct.
- Nginx logs revealed `403 Forbidden` errors for static file requests.

---

## Troubleshooting Steps

### 1. **Check Static File Permissions**

First, check the permissions of the static files (e.g., `tailwind.css`) and directories.

Run the following command to view the file permissions:

```bash
ls -l /home/ubuntu/Stock-App/app/static/css
````
```bash
-rwxr-xr-x 1 www-data www-data    59 Dec 24 10:23 input.css
-rwxr-xr-x 1 www-data www-data 19279 Dec 24 10:23 tailwind.css
````
The permissions rwxr-xr-x are correct, allowing read and execute access for all users. These permissions should be sufficient for Nginx to serve the files.

#2. Check Directory Permissions
Even though the file permissions were correct, the parent directories may not have the right permissions for Nginx to traverse them. Run the following command to ensure Nginx can access all parent directories:

```bash
sudo chmod 755 /home/ubuntu /home/ubuntu/Stock-App /home/ubuntu/Stock-App/app /home/ubuntu/Stock-App/app/static /home/ubuntu/Stock-App/app/static/css
```
This ensures that Nginx can read and execute from the entire directory path.
3. Check Nginx Configuration
Make sure that the Nginx configuration is set up correctly for serving static files. Specifically, the alias directive should be used in the location /static/ block to point to the correct directory.

Check your Nginx configuration (usually located at /etc/nginx/sites-available/default or /etc/nginx/nginx.conf):
```bash
location /static/ {
    alias /home/ubuntu/Stock-App/app/static/;
}
````
The alias directive points to the exact directory where your static files are stored (/home/ubuntu/Stock-App/app/static/).
Make sure there are no conflicting root directives.

4. Check SELinux (if applicable)
If you're using SELinux, it can sometimes block Nginx from accessing static files, even if the file system permissions are correct. Check whether SELinux is enabled:
```bash
sestatus
```
If SELinux is enabled, it might block Nginx from serving static files. To temporarily disable SELinux or ask Chat gpt he will help



