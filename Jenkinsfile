pipeline {
  agent any

  tools {
    maven 'maven-3'
  }

  environment {
    DOCKERHUB_CREDENTIALS= credentials('docker-hub-cred')
    DOCKERHUB_IMAGE= 'username/image:hello-world'
  }

  stages {
    stage('check maven version' ) {
      steps {
        script {
          sh '''
            mvn -v
            mvn clean install -T 1C
          '''
        }
      }
    }

    stage('Build Docker image') {
      steps{
        script {
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          sh "docker build -t hello-world ."
          sh "docker run -d -p 8080:8080 --name hello-world hello-world"
        }
      }
    }
    
    stage('Pushing to dockerhub registry') {
      steps {
        script {
          sh 'docker push $DOCKERHUB_IMAGE'
        }
      }
    }
  }
}