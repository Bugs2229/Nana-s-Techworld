# Nana's Module-5 Reveiw 

## Topology 


## Overveiw
Module 5 is essentially a mini end-to-end deployment that involved the following:
* Spin up cloud infrastructure, and get it running publicly. 
* Create a firewall and assing the firewall to the droplet in the cloud. 
* Configure the firewall to allow inbound port over TCPs:80|22|443|3000 from local laptop to server.
* Take a Node.js app and package it on local laptop.
* Copy the Node.js app to Droplet in the cloud.
* Run the app in the directory you selected on the droplet.
* Create a user account to use instead of the root user.

## Test
Take the following steps to verfy that the configuration is working.
* Open browser and type in http://server's ip address:3000. If a web page  comes up, exercise was completed correctly. 

#Steps

##Step 1

# clone repository & change into project dir
git clone git@gitlab.com:twn-devops-bootcamp/latest/05-cloud/cloud-basics-exercises.git
cd node-project

### remove remote repo reference and create your own local repository
rm -rf .git
git init 
git add .
git commit -m "initial commit"

### create git repository on Gitlab and push your newly created local repository to it
git remote add origin git@gitlab.com:{gitlab-user}/{gitlab-repo}.git
git push -u origin master

##Step 2

cd app
npm pack

##Step 3

### ssh into your newly created server
ssh root@{server-ip-address}

### install nodejs and npm
apt install -y nodejs npm

##Step 4

### secure copy project file from local machine to server. Execute from project's root folder.
scp bootcamp-node-project-1.0.0.tgz root@{server-ip-address}:/root

##Step 5

### ssh into droplet
ssh -i ~/id_rsa root@{server-ip-address}

### unpack the node-project file
tar -zxvf bootcamp-node-project-1.0.0.tgz

### change into unpacked directory called "package"
cd package

### install dependencies
npm install

### run the application
# Nana's Module-5 Reveiw 

## Topology 
![Alt text](https://gitlab.com/nana_techworld/nana_techworld/-/blob/84853f657e65b608c74c8f96be577f4b8e085a7f/Images/Module_5.gif)

## Overveiw
Module 5 is essentially a mini end-to-end deployment that involved the following:
* Spin up cloud infrastructure, and get it running publicly. 
* Create a firewall and assing the firewall to the droplet in the cloud. 
* Configure the firewall to allow inbound port over TCPs:80|22|443|3000 from local laptop to server.
* Take a Node.js app and package it on local laptop.
* Copy the Node.js app to Droplet in the cloud.
* Run the app in the directory you selected on the droplet.
* Create a user account to use instead of the root user.

## Test
Take the following steps to verfy that the configuration is working.
* Open browser and type in http://server's ip address:3000. If a web page  comes up, exercise was completed correctly. 

#Steps

##Step 1

# clone repository & change into project dir
git clone git@gitlab.com:twn-devops-bootcamp/latest/05-cloud/cloud-basics-exercises.git
cd node-project

### remove remote repo reference and create your own local repository
rm -rf .git
git init 
git add .
git commit -m "initial commit"

### create git repository on Gitlab and push your newly created local repository to it
git remote add origin git@gitlab.com:{gitlab-user}/{gitlab-repo}.git
git push -u origin master

##Step 2

cd app
npm pack

##Step 3

### ssh into your newly created server
ssh root@{server-ip-address}

### install nodejs and npm
apt install -y nodejs npm

##Step 4

### secure copy project file from local machine to server. Execute from project's root folder.
scp bootcamp-node-project-1.0.0.tgz root@{server-ip-address}:/root

##Step 5

### ssh into droplet
ssh -i ~/id_rsa root@{server-ip-address}

### unpack the node-project file
tar -zxvf bootcamp-node-project-1.0.0.tgz

### change into unpacked directory called "package"
cd package

### install dependencies
npm install

### run the application
node server.js


# Note

What is Node.js: Node.js is a runtime environment that lets you run JavaScript outside of a web browser — on servers, your local machine, wherever.

Why it matters in practice:

Backend development — you can build APIs and web servers entirely in JavaScript (Express is the classic framework for this)
Tooling — tons of dev tools run on Node, which is why you install things with npm (Node Package Manager). React projects, for example, need Node installed to build and run locally, even though React itself runs in the browser
One language everywhere — teams can use JavaScript for both frontend and backend

What is Gradle: 
Gradle is a build tool, primarily for Java projects (also Kotlin, Android, and others). It automates the process of turning your source code into a runnable application.

What "building" involves for a Java project:

Compiling — turning .java source files into bytecode the JVM can run
Dependency management — downloading the libraries your project needs (similar to what npm does for Node.js)
Testing — running your test suite automatically
Packaging — bundling everything into a deployable artifact like a .jar or .war file
