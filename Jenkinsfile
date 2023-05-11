pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '780888728956.dkr.ecr.us-east-1.amazonaws.com/devops-terra'
    registryCredential = 'aws-credentials'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/dzidzi1984/maven-test1.git'
            }
        }
        stage(sonarqube scan){
          steps{
          withSonarQubeEnv('sonar'){
          sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=dzidzi1984_geolocation3'
          }
            
  
          }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean install package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
