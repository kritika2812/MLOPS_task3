AUTOMATION OF MACHINE LEARNING (DL)
  VARIOUS TOOLS ARE USED IN THIS TASK :-

1. Git and Github- for version control and hosting our repository.
2. Jenkins- to automate various jobs.
3. Rhel8- as a base os for running services like httpd, jenkins,ngrok.
4. Docker- to run our python model.

Task Description:-

1. Create a container image that’s has Python3 ,Keras ,numpy and all the required libraries installed using dockerfile
When we launch this image, it should automatically starts train the model in the container.

2.Create a job chain of job1, job2, job3, job4 and job5 using build pipeline plugin in Jenkins

3.Job1 : Pull the Github repo automatically when some developers push repo to Github.

4.Job2 : By looking at the code or program file, Jenkins should automatically start the respective machine learning software installed, Interpreter install image container to deploy code and start training.

5.Job3 : Display the Accuracy in webbrowser.

6.Job4 : if metrics accuracy is less than 88% , then tweak the machine learning model architecture.

7.Job5: Retrain the model or notify that the best model is being created.

8.Job6: Will notify the developer that tweaking is started as model accuracy is less than required.

9.Create One extra job job7 for monitor : If container where app is running. fails due to any reason then this job should automatically start the container again from where the last trained model left


Lets begin the task:
- first create the workspace in your desktop names as mlops_task3 and copy all the codes in the form of files.
- open your git bash write the following commands :
     - cd Desktop
	 - cd mlops_task3
	 - git init 
	 - git add .
	 - git commit . -m "first commit"
	 - git remote add origin (your github url)
	 - git push -u origin master
	 
	 
- 	Now setting up your ngrok tunnel	
    We set up ngrok software for the port and protocol Jenkins is running. In my case Jenkins is running on port 8080 and using http protocol. Use command-
	
	./ngrok http 8080
	
	We will use this ip provided by ngrok in setting up our web-hooks in Github repo.
	
	
 - Setting Web-hooks in Github Repo-
 ->Settings ->in Options ->Webhooks ->Add Webhook ->Enter your password ->In payload url enter [ngrok_ip/github-webhook/] ->Change Content type to [application/json] ->then Save    the Webhook



- NOW CREATE JOB 1 IN JENKINS:-
  JOB1 will automatically pull the Github repo to a specified directory in Rhel Os whenever developer pushes any new code.
   
- NOW CREATE OWN CUSTOMIZED IMAGE FOR THE TASK
  To create Docker image first create a new folder named ws using-
  #mkdir mlopsws
  #cd mlopsws
  #gedit Dockerfile
   ** paste the following the code
   
   
   
   
   
   after saving this file. Write the following command 
   # docker build -t task3:v1(now image is tagged with version(v1)
   
   
 - JOB 2 :
 By looking at the code or program file, automatically start the respective machine learning software installed, Interpreter install image container to deploy code and start training
 
 
 - JOB 3 :
 First start the services of webserver in base OS
 #systemctl start httpd
 Set Build Triggers to ->Build after other Projects are build->put JOB2 in it.
 Paste the following code in build shell
 ***
 After successful build of JOB3, We can see show_output.html file in our local web-browser.
 
 
 
 - JOB 4:
 In build triggers , write job 3 to watch 
 now, paste the following code in build shell
  ***
  
  As JOB4 will run again and again until desired accuracy is not acheived, Our show_display.html file will also get changed by JOB3 and will show different accuracies and hyperparameters from each run of JOB2 model.
 
 
 - JOB 5:
 After getting the desired accuracy from JOB2 model, JOB4 will get terminated and JOB5 will run. This job will run model_success_mail.py and send the notification to the developer.
 
 In build triggers , write job 4 to watch.
 now, paste the following code in build shell
 *****
 
 - JOB 6:
 when JOB2 model does not acheived the desired accuracy, Then JOB4 will run JOB2 and JOB6. JOB6 will send the message of unsuccess and running the tweaking code by running model_lessaccuracy_mail.py.
 
 
 - JOB 7:
 JOB7 will run Every Hour and check If Container where model is running, Fails due to any reason then this job should automatically start the container again from where the last trained model left.
 
 In build triggers , click on poll Scm and write the syntax -> H * * * *
 







    	
	
	
	  
	 
	 
