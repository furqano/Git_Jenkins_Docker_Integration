
# Github - Jenkins - Docker 

## This is a project to automate end-end process using Jenkins & Docker

## requirements (pre-installed)

 * Github
 * Jenkins
 * Docker 
 
## Github bash :
 
 Im using the git clone method to simplifiy the steps .
 
 ```
 git clone <repository link>
 ```
 
 ![git repository](./images/08.jpg)
 
 ![git bash](./images/01.png)
 
 * I have used two branch master (created by default) & dev1 (maually created )
 
 * master branch is deployed in the production system (Docker image)
 
 * dev1 branch is deployed in the testing system (Docker image)
 
 * when the job3 run the dev1 branch merges with master and gets deployed to the production system .
 
### Creating git hooks post-commit 
 
 ```
$ notepad .git/hooks/post-commit
``` 
* Paste the below script in the notepad
```
#! bin/bash

git push
```
 Note : The .git/hooks/post-commit is global for the working repository no need of creating it for every branch . 
 
# creating the git-webhook
 
 ```
 repository setting -> webhooks -> add webhook -> paste the <url/github-webhook/>
 ```
![webhooks](./images/07.png)

## ngrok

* Github-webhooks requires a public ip to push in order to hook jenkins im using ngrok to tunnel to a public ip  

* you have to download the software & run :

```
./ngrok
```

## configuring the jenkins

 To start the jenkins 
 
 ```
 systemctl start jenkins
 ```
 
 Note : jenkins usually run on port 8080 i.e, ip:8080/
 
 This is the homepage of jenkins
 
 ![jenkins home](./images/02.png)
 
 To create a job click on new item 
 
 ![job creation](./images/03.jpg)
 
 *Note :make sure to select the freestyle project*
 
 ## job1
 
 Creating the job1 (master) :
 
  ![job1](./images/05.jpg)
  
  * make sure to select the -> **triggers -> GitHub hook trigger for GITScm polling**
  
  ![jenkins hooks](./images/04.jpg)
  
  **In build -> add build setup -> execute shell**
  
  * Paste this commands in the execute shell
  
  ```
sudo cp * /root/devops/

if sudo docker ps | grep dev
then
echo "already running "
else 
sudo docker run -d -it -v /devops:/usr/local/apache2/htdocs/ --name dev httpd
fi
``` 

 ## job2
 
 Creating the job2 (dev1) :
 
 ![job2](./images/06.jpg)
 
 * make sure to select the -> **triggers -> GitHub hook trigger for GITScm polling**
 
 ![jenkins hooks](./images/04.jpg)
 
 **In build -> add build setup -> execute shell**
 
 * Paste this commands in the execute shell
 
 ```
sudo cp * /root/devops/

if  sudo docker ps | grep dev1
then
echo "already running "
else 
sudo docker run -d -it  -v /devops:/usr/local/apache2/htdocs/ --name dev1 httpd
fi
``` 
Note : if needed can add **-p (any port)** i.e, 8080:80 to tunnel it through the base os ip.

## job3
 
 In **source code Management** there is an **Additional Behaviours** in that select the **post merge** 
 
 In the **Branch to merge to** : **master**
 
 ![job3](./images/16.jpg)
 
 Thats it ..Done !!
 
 Apply and Save 
 
## Invalid username and Password 
 
 * you might face an error of invalid username and password 
 
 To avoid this error create a token and use it a password while merging the branch (job3)
 
 ```
 settings -> Developer settings -> create a personal acess tokens 
 ```

## Docker
 
 To download the apache webserver image from docker hub .
 ```
 docker pull httpd
 ```
 Note : if no version of the image is given it selects **:latest** as default  

 The Docker images launched each container has its own dedicated ip adress . 
 
 To get the container ip adress we can use 
 
 ```
 docker inspect <container name>
 ```
 
 ![containerip](./images/14.jpg)
 
 
 The jobs created at background and the webserver is configured 
 
 ![docker](./images/10.jpg)
 
 ## Output :
 
 * Jenkins after all the jobs execute sucessfully 
 
 ![jenkins](./images/15.jpg)
 
 * Job1 output with it dedicated server 
 
 ![job1](./images/11.jpg)
 
 * Job2 output with it dedicated server 
 
 ![job2](./images/12.jpg)
 
 * Job3 output with the same server as job1 (production system)
 
 ![job3](./images/13.jpg)
 


