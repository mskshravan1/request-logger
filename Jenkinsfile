pipeline {
  agent any

  tools {
    maven 'mvn'
  }

  stages {
    stage('Build App') {
      steps {
        sh 'mvn package'
      }
    }
    
    stage('Build Image') {
      steps {
      sh "docker build -t mskshravan1/request-logger:${env.BUILD_ID} ."
      sh "docker tag brainupgrade/request-logger:${env.BUILD_ID} mskshravan1/request-logger:latest"
      }
    }
    
  }

  post {
    always {
      archive 'target/**/*.jar'
    }
    success {
      withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
        sh "docker login -u ${USERNAME} -p ${PASSWORD}"
        sh "docker push brainupgrade/request-logger:${env.BUILD_ID}"
        sh "docker push brainupgrade/request-logger:latest"
      }
    }
  }
}
