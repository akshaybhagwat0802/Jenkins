### Setup for jenkins and sonarqube
#### 1) launch 2 EC2 instances having instance type (c7i-flex.large) and name is jenkins or sonarqube.
#### 2) connect to the jenkins instance and do following
```bash
sudo -i
sudo apt update
sudo apt install openjdk-17-jdk -y
sudo apt install maven -y
```
install jenkins in server
```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins -y
```
#### 3) Acess the jenkins on port 8080 and do the configuration and setup.
#### 4) connect to the sonarQube server and do following
```bash
sudo -i
sudo apt update
sudo apt install maven -y
sudo apt install docker.io -y
```
Run the docker container 
```bash
docker run -d --name sonarqube-custom -p 9000:9000 sonarqube:10.6-community
```
check the container
```bash
docker ps
```
#### 5) Acess the port on port no 9000 with server url (for ex : http://url:9000)
```bash
http://130.67.98.102:9000/
```
default username: admin
default password: admin

#### 6) Sonarqube setup and configuration
 1) Setup of webhook <br>
     Administartion -->configuration --> webhook --> create <br>
     webhook name --> ```Sonar-webhook``` <br>
     url --> ``` http://130.24.97.102/sonarqube-webhook/ ```

 2) Create a project in sonarqube .
      project--> create local project <br>
      project name -``` studentapp```<br>
      project key - ``` studentapp```<br>
      branch - ```main```<br>
      click on next and select ```use global setting```<br>
      click on next then click on ```locally ``` then click on generate token<br>
      copy the token and save into the notepad (for ex: ```'sqp_c4a60a5eaf83935f092fd7730ab843fc311c7bd5'```)<br>
      copy the maven code and save into the notepad .
      
#### 7) jenkins setup and configuration
  1) install the plugins
     Jenkins Dashboard --> manage jenkins --> plugins --> available plugins --> ```SonarQube-scanner```
  2) configure the credential
     Dashboard --> manage jenkins --> credential <br>
     type of credential --> ```secrete txt``` <br>
     secrete --> ```<sonarqube secrete token>```<br>
     id --> ```soanr-token```<br>
  3) System configuration or add sornarqube server
     Dashboard --> manage jenkins --> system <br>
     Find sonarqube server section <br>
     Enable environment variables<br>
     add new sonarqube :
       - Name : ```Sonar-env```<br>
       - server url : ``` http://130.98.20.108:9000/```<br>
       - authenticate token : ```sonar-token``` <br>
     Restart the jenkins server
#### 8) make the changes into the pom.xml  add soanrqube dependency in the github project and save 

#### 9) create a new project in jenkins and create a pipeline .
  Script pipeline 
  ```bash
pipeline {
    agent any

    stages {
        stage('clone repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Rohit-1920/EasyCRUD.git'
            }
        }

        stage('build') {
            steps {
                sh '''
                cd backend
                mvn clean package -DskipTests
                '''
            }
        }

        stage('sonar analysis') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'Sonar-env') {
                    sh '''
                    cd backend
                    mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar \
                      -Dsonar.projectKey=studentapp \
                      -Dsonar.projectName=studentapp
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage('deploy') {
            steps {
                echo 'deployment stage'
            }
        }
    }
}

```
#### 10) if you found any issue or failure chack the token name and configuration is correct or not .
     
     

          

