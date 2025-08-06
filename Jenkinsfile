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
      steps {
        script {
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          sh "docker build -t hello-world ."
          // Tagging the image for DockerHub
          sh "docker tag hello-world $DOCKERHUB_IMAGE"
        }
      }
    }

    stage('Running docker image') {
      steps {
        sh '''
        docker rm -f hello-world-test || true
        docker run -d -p 8081:8080 --name hello-world-test hello-world
        '''
      }
    }
    
    stage('Pushing to dockerhub registry') {
      steps {
        script {
          sh "docker push ${DOCKERHUB_CREDENTIALS_USR}/${DOCKERHUB_IMAGE}"
        }
      }
    }
  }
}
