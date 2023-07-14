pipeline {
    agent any 
      stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/Birbalsarva/node-todo-cicd.git", branch: "main"
            }
        }
        stage('Build and Push Docker Image') {
        environment {
        DOCKER_IMAGE = "birbalsarva/node-todo-cicd:${BUILD_NUMBER}"
        DOCKERFILE_LOCATION = "node-todo-cicd/Dockerfile"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
          sh 'docker build -t ${DOCKER_IMAGE} .'
          def dockerImage = docker.image("${DOCKER_IMAGE}")
          docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
            dockerImage.push()
          }
        }
      }
    }
    stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
