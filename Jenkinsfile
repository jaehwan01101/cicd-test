pipeline {
  agent any
  environment {
    strDockerImage = "kimjaekimjae/cicd-test:0.1"
  }
  stages {
    stage('Github Pull') {
      steps {
        git branch: 'main', url:'https://github.com/jaehwan01101/cicd-test.git'
      }
    }
    stage('Docker Image Build') {
      steps {
        script {
          oDockImage = docker.build(strDockerImage,"-f Dockerfile .")
        }
      }
    }
    stage('Deploy Server') {
      steps {
        sshagent(credentials: ['Deploy-Privatekey']) {
          sh "scp -o StrictHostKeyChecking=no index.html ubuntu@43.201.48.51:/home/ubuntu/"
          sh "ssh -o StrictHostKeyChecking=no ubuntu@43.201.48.51 sudo cp /home/ubuntu/index.html /var/www/html"
        }
      }
    }
  }
}
