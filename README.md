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
 </ol>
  </P>
 </div>
</body>
</html>
