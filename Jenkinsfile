import java.text.SimpleDateFormat
def TODAY = (new SimpleDateFormat('yyyymmdd')).format(new Date())

pipeline {
  agent any
  environment {
    strDockerTag = "${TODAY}_${BUILD_ID}"
    strDockerImage = "kimjaekimjae/cicd-test:${strDockerTag}"
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
    stage('DOkcer Image Push') {
      steps {
        script {
          docker.withRegistry('', 'docker-auth') {
            oDockImage.push()
          }
        }
      }
    }
    stage('Deploy Server') {
      steps {
        sshagent(credentials: ['Deploy-Privatekey']) {
          sh "ssh -o StrictHostKeyChecking=no ubuntu@43.201.48.51 docker container rm -f sampleweb "
          sh "ssh -o StrictHostKeyChecking=no ubuntu@43.201.48.51 docker run -d -p 80:80 --name sampleweb ${strDockerImage}"
        }
      }
    }
  }
}
