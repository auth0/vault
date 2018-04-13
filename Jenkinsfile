pipeline {
  agent {
    label 'crew-brokkr'
  }

  options {
    buildDiscarder(logRotator(numToKeepStr:'5'))
    timeout(time: 30, unit: 'MINUTES')
  }

  parameters { 
    string(name: 'TAG_NAME', defaultValue: 'latest', description: 'Tag name for the image that is going to be pushed')
  }

  stages {
    stage('Build') {
      steps {
        sh "docker build . -t auth0brokkr/vault:${params.TAG_NAME}" 
      }
    }

    stage('Push') {
      environment {
        DOCKER_USER = credentials('docker-hub-user')
        DOCKER_PASSWORD = credentials('docker-hub-password')
      }
      steps {
        sh "docker login -u ${env.DOCKER_USER} -p ${env.DOCKER_PASSWORD}"
        sh "docker push auth0brokkr/vault:${params.TAG_NAME}"
      }
    }

    stage('Clean') {
      steps {
        sh "docker rmi auth0brokkr/vault:${params.TAG_NAME}"
      }
    }
  }

  post {
    always {
      deleteDir()
    }
  }
}
