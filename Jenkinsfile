pipeline {
  agent any
   environment {
       registry = "serhii1998/javatest"
       credentials = "dockerhub"
   }
   tools {
      jdk 'jdk8'
      maven 'maven3.6.0'
   }
   stages {
      stage('Clonning from GitHub') {
         steps {
            git credentialsId: 'git-cred', url: 'https://github.com/serhii1998/hello-world.git'
         }
      }
      stage('compile') {
          steps {
              sh 'mvn clean compile'
          }
      }
      stage('test') {
          steps {
              sh 'mvn test'
          }
      }
      stage('package war ') {
          steps {
              sh 'mvn package'
              }
      }
      stage('Build image') {
          steps {
              script {
                 myimage = docker.build(registry + ":v$BUILD_NUMBER") 
              }
              
          }
      }
      stage('Run(test) image') {
          steps {
              script {
                 sh 'docker run -d --name jenkins_husak -p 8090:8080 "$registry:v$BUILD_NUMBER" '
                 sh 'sleep 20'
                 sh 'curl http://localhost:8090/webapp/'
                 sh 'docker stop jenkins_husak '
                 sh 'docker rm jenkins_husak '
              }
          }
      }
    }
    
    post {
        always {
            deleteDir()
        }
    }
}
