### Creation of jenkins pepline
#### 1). Launch an EC2 instance having instance type c7i-flex.large.
#### 2) Connect the instance and do following.
```bash
sudo -i
```
```bash
sudo apt update
```
#### 3) Install java on the instance (java version is 17).
```bash
sudo apt install openjdk-17-jdk -y
java -- version
```
#### 4) open the google and search for jenkins and select the linux and select debian/ubuntu.

<img width="1920" height="1080" alt="Screenshot (145)" src="https://github.com/user-attachments/assets/731efe1c-9f27-4716-8e67-6bf6526a9428" />

#### 5) install the jenkins into the server 
```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins
```
#### 6) After installation copy the public ip of server and paste in chome and hit the jenkis port no (8080)
```bash
http://13.53.193.15:8080/
```
#### 7) copy the path which is shown in jenkins window and read the path on server and copy the password and paste in the jenkins window in password holder
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```
#### 8) select install custom plugins and give the username and password .click on the save and continue then click next and start the jenkins .
#### 9) click on create new item or create a job
#### 10) Give the new item and give the name select pipeline or free style project.
#### 11) select the github project or other you want create.
#### 12) create connection and configuration between github and jenkins 
1) open github.
2) select the repository which you have to clone.
3) go to setting of that repo and select webhook.
4) give the name to webhook and paste the jenkins url in the url tag
   ```bash
   http://130.23.76.105:8080/github-webhook/
   ```
   <img width="1920" height="1080" alt="Screenshot (147)" src="https://github.com/user-attachments/assets/406bcbdc-93d2-46dd-b1a2-e199f357c639" />

#### 12) add the stages as per project need 
```bash
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // git branch: 'main', url: 'https://github.com/akshaybhagwat0802/jenkins-demo.git'
            }
        }
        stage('Test') {
            steps {
                // sh'''
                   cd backend/
                   mvn clean package -DskipTests
            }
        }
        stage('Deploy') {
            steps {
                // echo 'deploy'
            }
        }
    }
}
```
#### 13) click on the build now and build 
#### 14) Add the plugin for stage view 
       
