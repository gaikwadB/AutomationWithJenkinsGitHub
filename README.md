# Automation With Jenkins, Git, GitHub and DevOps
This task is given by Vimal Daga Sir,in this project i implement the automation infrastructure integrating with Jenkins,Git and GitHub.

## Project Description:

  > 1.Create container image that’s has Jenkins installed using dockerfile.
  
  > 2. When we launch this image, it should automatically starts Jenkins service in the container.
  
  
  > 3. Create a job chain of job1, job2, job3 and job4 using build pipeline plugin in Jenkins.
  
  
  >  4. Job1 : Pull the Github repo automatically when some developers push repo to Github.
  
  
  >  5. Job2 : By looking at the code or program file, Jenkins should automatically start the respective language interpreter install image    container to deploy code ( eg. If code is of PHP, then Jenkins should start the container that has PHP already installed ).
  
  
  > 6. Job3 : Test your app if it is working or not. If app is not working , then send email to developer with error messages.
  
  
  > 7. Job4 : If container where app is running. fails due to any reson then this job should automatically start the container again.
  
  
  Before we implement this project we should have the following prerequisites.
  
 ## Prerequisites
 
 * OS: Base OS is Windows 10. Server OS is RedHat Enterprise Linux 8 (RHEL8) in Virtual Box.
 
 *  In RHEL8 some of the softwares needed are Docker (also need the php image downloaded in it), Jenkins (also github, build pipeline and email extension plugin should be installed in it).
 
* In Windows we need git bash software.

* At the starting stop the firewalld in RHEL8 and start the docker and jenkins services.

## Step By Step Procedure Of Creating This Project:

## 1. Creating Dockerfile and Building Personal Image using it :

   *Just see the below mentioned picture. Here I create one Dockerfile and run the build command. Remember one thing you have to run          this command particularly on that folder where you Dockerfile exits.
       
   * Here i have created image using following commands
         
         docker build -t myjen:v1 .
         
   * Here , i have define the code of Dockerfile:
      
          FROM centos:8

          RUN yum install wget -y
          RUN wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
          RUN rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
          RUN yum install java -y && yum install jenkins -y && yum install git -y && yum install sudo -y
          RUN echo -e "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

          CMD java -jar /usr/lib/jenkins/jenkins.war

          EXPOSE 8080
          
  :![dataset](/Screenshots/dockerfile.png)

## 2. Running the container and getting the files:
   
   In the below picture you can see that, docker image is creating with their required dependencies,packages etc.
      
  :![dataset](/Screenshots/building_image.png)
  
  
  After building the container, we can see in the below picture that we have created the one docker image file that name is myjetty:v1
  
  
  :![dataset](/Screenshots/image_done.png) 
  
  Once we creted a docker file you can run the container based on that myjeety:v1 image file.
  To run the container here is the command:
      
      docker run -it -P --previlleged -v /:/host --name myjen myjetty:v1
  
  :![dataset](/Screenshots/docker_running_process.png)
  
  Once these process done, then you have to run the jenkins by putting the ip address of base redhat os with port allocated to the myjen  container in webbrowser.
  
 To unclock the jenkins you have to paste the password, to get the password put the following command in terminal
 
      docker exec myjen cat /root/.jenkins/secrets/InitialAdminPassword
      
## 3. Jenkins Job1 :

   This job pull the Github repo automatically when some developers push repo to Github.
   Follow the below mentioned picture and build the 1st job in Jenkins.
   
   Go to the 'configure' of your job and fill these data. First give the URL of your project repository. Next in SCM select git and         fill the repo URL and select the branch to dev.
    
   And in build trigger just select Build Trigger Remotely and give a desired token name. This will help us to trigger jenkins as soon      as we push our code to github remotely.
   :![dataset](/Screenshots/job1_01.png)
   :![dataset](/Screenshots/job1_02.png)
   
   * Next this is the code which gonna help to copy our files from Jenkins Workspace to our desired folder.
   
   :![dataset](/Screenshots/job1_03.png)
   
   
## 4.  Jenkins Job2 :

  Follow the below mentioned picture and build the 2nd job in Jenkins.
  This job do this Jenkins should automatically start the respective language interpreter install image container to deploy code ( eg.     If code is of PHP, then Jenkins should start the container that has PHP already installed ).
  
  :![dataset](/Screenshots/job2_01.png)
  :![dataset](/Screenshots/job2_02.png)
  
## 5. Jenkins Email Configuration :

  Follow the below mentioned picture and configure your email address with Jenkins to get notifications :
  Goto The Manage Jenkis > Configure System and scroll down and fill up the email notification portion
  
  :![dataset](/Screenshots/email_configuration.png)
  
## 6. Jenkins Job 3:
  
  Follow the below mentioned picture and build the 3rd job in Jenkins.
  Test your app if it is working or not. If app is not working , then send email to developer with error messages.
  Here again use the previous trigger. This code will check if our php code is working fine or not.

  :![dataset](/Screenshots/job3_01.png)
  :![dataset](/Screenshots/job3_02.png)
  :![dataset](/Screenshots/job3_03.png)
  
## 7. Jenkins Job4 :

  Follow the below mentioned picture and build the 3rd job in Jenkins.
  This job will keep on checking each minutes if my docker container is running or not.These 5 start means each minute this job will        automatically triggers itself
  
  :![dataset](/Screenshots/job4_01.png)
  :![dataset](/Screenshots/job4_02.png)
 
## 8.Creating Build Pipeline View :
  Just simply create one new My view in Jenkins and select Build Pipeline. Then at the end only mention job1 and you are done.

## we looked on the result

:![dataset](/Screenshots/build_pipeline_successfull.png)
:![dataset](/Screenshots/job_run_ suceesful.png)
:![dataset](/Screenshots/job_running.png)
:![dataset](/Screenshots/webpage_host.png)

once we changed the content of webpage and their will be some error you will get following result

:![dataset](/Screenshots/build_pipeline_failed.png)
:![dataset](/Screenshots/webpage_problem.png)
:![dataset](/Screenshots/email.png)

  
  
      

