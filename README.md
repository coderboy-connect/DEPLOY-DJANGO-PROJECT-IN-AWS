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
</div>
</body>
</html>
