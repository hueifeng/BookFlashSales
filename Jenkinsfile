pipeline {
 agent {
    node {
      label 'base'
    }
  }

  stages {
 stage('Clone repository') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: 'main']], 
                    extensions: [[$class: 'CloneOption', depth: 1, shallow: true]], userRemoteConfigs: [[url: 'https://github.com/hueifeng/BookFlashSales']]
                ])
      }
    }
    stage('build & push') {
      agent none
      steps {
        container('base') {
          sh 'echo $DOCKER_HUB | docker login -u hueifeng --password-stdin'
          sh 'git clone https://github.com/hueifeng/BookFlashSales'
          sh 'ls'
          sh 'cd BookFlashSales && podman build -f src/BookFlashSales.Web/Dockerfile .'
          sh 'docker push hueifeng/bookflashsales-api:latest'
        }

      }
    }

  }
   environment {
        REGISTRY = 'easy'
        DOCKERHUB_USERNAME = 'ks-devops-harbor'
        HARBOR_NAMESPACE = 'ks-devops-harbor'
        APP_NAME = 'easy-java'
        DOCKER_HUB= credentials('docker')
      }
}