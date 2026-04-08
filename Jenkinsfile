pipeline {
  agent any

  stages {
    stage('Github Pull') {
      steps {
        git branch: 'main', url:'https://github.com/jaehwan01101/cicd-test.git'
      }
    }
    stage('Git clone end') {
      steps {
        sh 'touch  cicd_test.txt'
        sh 'echo "git clone end" > cicd_test.txt'
      }
    }
    stage('Deploy Server') {
      steps {
        sshagent(credentials: ['DeplFoy-Privatekey']) {
          sh "scp -o StrictHostKeyChecking=no index.html ubuntu@43.201.48.51:/home/ubuntu/"
          sh "ssh -o StrictHostKeyChecking=no ubuntu@43.201.48.51 sudo cp /home/ubuntu/index.html /var/www/html"
        }
      }
    }
  }
}
