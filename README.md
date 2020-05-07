
# Github - Jenkins - Docker 

## This is a project to automate end-end process using Jenkins & Docker

## requirements

 1. Github
 2. Jenkins
 3. Docker 
 
 ## Github bash :
 
 * I have used two branch master (created by default) & dev1 (maually created )
 * master branch is deployed in the production system (Docker image)
 * dev1 branch is deployed in the testing system (Docker image)
 
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
 
 ## configuring the jenkins
 ![Algorithm schema](./images/schema.jpg)
 ![Algorithm schema](./images/schema.jpg)
 ![Algorithm schema](./images/schema.jpg)
 ![Algorithm schema](./images/schema.jpg)
 
## Setup
To run this project, install it locally using npm:

```
$ cd ../lorem
$ npm install
$ npm start
``` 
