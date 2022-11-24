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
        'NAME': 'YOUR_DATABASE_NAME',
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
Create an administrative user if you are using inbuild django user. Else continue....
````
We can collect all of the static content into the directory location we configured in our project settings by typing:
````
~/myproject/manage.py collectstatic
````
You will have to confirm the operation. The static files will then be placed in a directory called static within your project directory.

If you followed the initial server setup guide, you should have a UFW firewall protecting your server. In order to test the development server, we’ll have to allow access to the port we’ll be using.

   
# changing the firewall settings
This settings make sure that which protocols or who all can access our site.
Create an exception for port 8000 by typing:
````
sudo ufw allow 8000
````

Testing Gunicorn’s Ability to Serve the Project
The last thing we want to do before leaving our virtual environment is test Gunicorn to make sure that it can serve the application. We can do this by entering our project directory and using gunicorn to load the project’s WSGI module:

````
cd ~/myproject
gunicorn --bind 0.0.0.0:8000 myproject.wsgi
````
This will start Gunicorn on the same interface that the Django development server was running on. You can go back and test the app again.

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
copy and past the code given below to that file.
Don't forget to change the User name sammy to your username. you can check it by typing pwd. and look the folder after home. Click this <a href = '' > link </a> to watch how it is done.
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

[Install]
WantedBy=multi-user.target
````
   
# With that, our systemd service file is complete.
 save and close it now.

We can now start the Gunicorn service we created and enable it so that it starts at boot:
````
sudo systemctl start gunicorn
````
````
sudo systemctl enable gunicorn
````
````
sudo systemctl daemon-reload
````
sudo systemctl restart gunicorn
````
Make sure you troubleshoot any of the above issues before continuing.

# Configure Nginx to Proxy Pass to Gunicorn
Now that Gunicorn is set up, we need to configure Nginx to pass traffic to the process.

Start by creating and opening a new server block in Nginx’s sites-available directory:

````
sudo nano /etc/nginx/sites-available/myproject
````
Copy and past the below code in it.

````
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
   
````
Save and close the file when you are finished. Now, we can enable the file by linking it to the sites-enabled directory:

````
sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
````   

Complete your nginx work by entering 

````
sudo systemctl restart nginx
````
Finally, we need to open up our firewall to normal traffic on port 80. Since we no longer need access to the development server, we can remove the rule to open port 8000 as well:

````
sudo ufw delete allow 8000
````
````
sudo ufw allow 'Nginx Full'
`````
 </ol>
  </P>
 </div>
</body>
</html>
