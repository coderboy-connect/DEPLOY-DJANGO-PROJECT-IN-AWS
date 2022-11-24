# DEPLOY-DJANGO-PROJECT-IN-AWS

Today we are going to see how to deploy our django project in aws.

<html>
<body>
<div>
 
  <p>
    step 1 : make sure your project is working well when you are running it in a local server
  </p>
  <p>
    step 2 : create a file called requirement.txt by typing the command 'pip freeze > requirement.txt' in your terminal. make sure you are in the project folder.
  </p>
   <p>
    step 3 : go to your project settings file and change the things shown below
    


```
...
...
ALLOWED_HOSTS = ['*']
...
...
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'project_wideYOUR_DATABASE_NAME',
        'USER':'USERNAME',
        'PASSWORD':'PASSWORD'
    }
}
```
  </p>
 <p>
 ( I hope your satic and media files are working perfectly fine  in your local host, so i am not going to say about handling them. )
  <p>
   
   <p>
    step 3 : now you create an account in github and make a repo then push all your project file into that repository. you can use either github desktop or sourcetree or git bash , etc to complete this task.
 </P>
 <h3>Getting an account in aws</h3>
 <p>
  use this link to go to the aws console <a href='https://aws.amazon.com/'>AWS(coderboy)</a>
 </p>
 <p>
  step 4 : now you have to create an aws account as a root user using your valid information. <a href=''>Click me</a> to know about creating an Aws account in Malayalam.
 </p>
 <p>
  step 5 : after creating the aws account you will be redirected to the aws console. now you have to search for ec2 on the search bar. 
 </P>
 <p>
 step 6 : now you have to buy an instance so that you can host your site in it.  <a href=''>Click me</a> If you want to see how step 5 and step 6 are done.
 </p>
 <p>
  step 7 : Now you have to establish a connection with your instance and make sure that your instance is upto date , follow this <a href=''>link</a> to see that.
 </p>
 <p>
  step 8 : now you have to type the comments that is listed below one by one .
  <ol>
  
THe following commands will work only if you are using pythoon 3. you can check your python version by typing 'python --version' in your cmd. just make sure that the version starts with 3 . ex: Python 3.10.5
   
enter this comment to get the latest pip features
   
````
sudo -H pip3 install --upgrade pip
   
````
Install the virtual environment for your project
````
sudo -H pip3 install virtualenv
`````
With virtualenv installed, we can start forming our project. Create and move into a directory where we can keep our project files:
you can use your project name instead of 'myproject' if you want.
`````
mkdir ~/myproject
cd ~/myproject
`````
Within the project directory, create a Python virtual environment by typing:
you can choose the name of the environment , i am using the the name as myprojectenv
`````
virtualenv myprojectenv
   
`````
This will create a directory called myprojectenv within your myproject directory. Inside, it will install a local version of Python and a local version of pip. We can use this to install and configure an isolated Python environment for our project.

Before we install our project’s Python requirements, we need to activate the virtual environment. You can do that by typing:
dont forget to use your environment name in the place of myprojectenv.
`````
source myprojectenv/bin/activate
`````
Your prompt should change to indicate that you are now operating within a Python virtual environment. It will look something like this: (myprojectenv)user@host:~/myproject$.

With your virtual environment active, install Django, Gunicorn, and the psycopg2 PostgreSQL adaptor with the local instance of pip:

Note

Regardless of which version of Python you are using, when the virtual environment is activated, you should use the pip command (not pip3).
   
````
pip install django gunicorn psycopg2
````
# You should now have all of the software needed to start a Django project.

Create and Configure a New Django Project
With our Python components installed, we can create the actual Django project files.

I believe that you have your project in your github. if not then just do that before we continue. or else lets go...
At this point, your project directory (~/myproject in our case) should have the following content:

~/myproject/manage.py: A Django project management script.
~/myproject/myproject/: The Django project package. This should contain the __init__.py, settings.py, urls.py, and wsgi.py files.
~/myproject/myprojectenv/: The virtual environment directory we created earlier.
   
Complete Initial Project Setup
Now, we can migrate the initial database schema to our mysql database using the management script:

````
~/myproject/manage.py makemigrations
~/myproject/manage.py migrate
````
Create an administrative user for the project by typing:
````
~/myproject/manage.py createsuperuser
````
You will have to select a username, provide an email address, and choose and confirm a password.

We can collect all of the static content into the directory location we configured by typing:
````
~/myproject/manage.py collectstatic
````
You will have to confirm the operation. The static files will then be placed in a directory called static within your project directory.

If you followed the initial server setup guide, you should have a UFW firewall protecting your server. In order to test the development server, we’ll have to allow access to the port we’ll be using.

Create an exception for port 8000 by typing:
````
sudo ufw allow 8000
````
Finally, you can test our your project by starting up the Django development server with this command:
````
~/myproject/manage.py runserver 0.0.0.0:8000
````
In your web browser, visit your server’s domain name or IP address followed by :8000:

http://server_domain_or_IP:8000
You should see the default Django index page:

Django index page

If you append /admin to the end of the URL in the address bar, you will be prompted for the administrative username and password you created with the createsuperuser command:

Django admin login

After authenticating, you can access the default Django admin interface:

Django admin interface

When you are finished exploring, hit CTRL-C in the terminal window to shut down the development server.

Testing Gunicorn’s Ability to Serve the Project
The last thing we want to do before leaving our virtual environment is test Gunicorn to make sure that it can serve the application. We can do this by entering our project directory and using gunicorn to load the project’s WSGI module:

````
cd ~/myproject
gunicorn --bind 0.0.0.0:8000 myproject.wsgi
````
This will start Gunicorn on the same interface that the Django development server was running on. You can go back and test the app again.

Note: The admin interface will not have any of the styling applied since Gunicorn does not know about the static CSS content responsible for this.

We passed Gunicorn a module by specifying the relative directory path to Django’s wsgi.py file, which is the entry point to our application, using Python’s module syntax. Inside of this file, a function called application is defined, which is used to communicate with the application. To learn more about the WSGI specification, click here.

When you are finished testing, hit CTRL-C in the terminal window to stop Gunicorn.

We’re now finished configuring our Django application. We can back out of our virtual environment by typing:
````
deactivate
````
The virtual environment indicator in your prompt will be removed.

Create a Gunicorn systemd Service File
We have tested that Gunicorn can interact with our Django application, but we should implement a more robust way of starting and stopping the application server. To accomplish this, we’ll make a systemd service file.

Create and open a systemd service file for Gunicorn with sudo privileges in your text editor:
````
sudo nano /etc/systemd/system/gunicorn.service
````
````
/etc/systemd/system/gunicorn.service
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=sammy
Group=www-data
WorkingDirectory=/home/sammy/myproject
ExecStart=/home/sammy/myproject/myprojectenv/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/sammy/myproject/myproject.sock myproject.wsgi:application
   
````
Finally, we’ll add an [Install] section. This will tell systemd what to link this service to if we enable it to start at boot. We want this service to start when the regular multi-user system is up and running:
````
/etc/systemd/system/gunicorn.service
````
   
````
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=sammy
Group=www-data
WorkingDirectory=/home/sammy/myproject
ExecStart=/home/sammy/myproject/myprojectenv/bin/gunicorn --access-logfile - --workers 3 --bind unix:/home/sammy/myproject/myproject.sock myproject.wsgi:application

[Install]
WantedBy=multi-user.target
````
   
With that, our systemd service file is complete. Save and close it now.

We can now start the Gunicorn service we created and enable it so that it starts at boot:

sudo systemctl start gunicorn
sudo systemctl enable gunicorn
We can confirm that the operation was successful by checking for the socket file.

Check for the Gunicorn Socket File
Check the status of the process to find out whether it was able to start:

sudo systemctl status gunicorn
Next, check for the existence of the myproject.sock file within your project directory:

````
ls /home/sammy/myproject
````
Output
manage.py  myproject  myprojectenv  myproject.sock  static
If the systemctl status command indicated that an error occurred or if you do not find the myproject.sock file in the directory, it’s an indication that Gunicorn was not able to start correctly. Check the Gunicorn process logs by typing:

sudo journalctl -u gunicorn
Take a look at the messages in the logs to find out where Gunicorn ran into problems. There are many reasons that you may have run into problems, but often, if Gunicorn was unable to create the socket file, it is for one of these reasons:

The project files are owned by the root user instead of a sudo user
The WorkingDirectory path within the /etc/systemd/system/gunicorn.service file does not point to the project directory
The configuration options given to the gunicorn process in the ExecStart directive are not correct. Check the following items:
The path to the gunicorn binary points to the actual location of the binary within the virtual environment
The --bind directive defines a file to create within a directory that Gunicorn can access
The myproject.wsgi:application is an accurate path to the WSGI callable. This means that when you’re in the WorkingDirectory, you should be able to reach the callable named application by looking in the myproject.wsgi module (which translates to a file called ./myproject/wsgi.py)
If you make changes to the /etc/systemd/system/gunicorn.service file, reload the daemon to reread the service definition and restart the Gunicorn process by typing:

sudo systemctl daemon-reload
sudo systemctl restart gunicorn
Make sure you troubleshoot any of the above issues before continuing.

Configure Nginx to Proxy Pass to Gunicorn
Now that Gunicorn is set up, we need to configure Nginx to pass traffic to the process.

Start by creating and opening a new server block in Nginx’s sites-available directory:

sudo nano /etc/nginx/sites-available/myproject
Inside, open up a new server block. We will start by specifying that this block should listen on the normal port 80 and that it should respond to our server’s domain name or IP address:

/etc/nginx/sites-available/myproject
server {
    listen 80;
    server_name server_domain_or_IP;
}
Next, we will tell Nginx to ignore any problems with finding a favicon. We will also tell it where to find the static assets that we collected in our ~/myproject/static directory. All of these files have a standard URI prefix of “/static”, so we can create a location block to match those requests:

/etc/nginx/sites-available/myproject
server {
    listen 80;
    server_name server_domain_or_IP;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/sammy/myproject;
    }
}
Finally, we’ll create a location / {} block to match all other requests. Inside of this location, we’ll include the standard proxy_params file included with the Nginx installation and then we will pass the traffic to the socket that our Gunicorn process created:

/etc/nginx/sites-available/myproject
server {
    listen 80;
    server_name server_domain_or_IP;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/sammy/myproject;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/sammy/myproject/myproject.sock;
    }
}
Save and close the file when you are finished. Now, we can enable the file by linking it to the sites-enabled directory:

sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
Test your Nginx configuration for syntax errors by typing:

sudo nginx -t
If no errors are reported, go ahead and restart Nginx by typing:

sudo systemctl restart nginx
Finally, we need to open up our firewall to normal traffic on port 80. Since we no longer need access to the development server, we can remove the rule to open port 8000 as well:

sudo ufw delete allow 8000
sudo ufw allow 'Nginx Full'
`````
 </ol>
  </P>
 </div>
</body>
</html>
