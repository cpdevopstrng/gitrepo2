pipeline {
  agent any
  stages {
    stage('SCM') {
      steps {
        git(url: 'https://github.com/cpdevopstrng/gitrepo2', branch: 'main', credentialsId: 'de269ee3-5055-4ade-9917-74c931bf7123')
      }
    }

    stage('Build&Publish') {
      steps {
        sh '''sudo docker build -t cpdevopstrng/srepo1:latest .
sudo docker push cpdevopstrng/srepo1:latest'''
      }
    }

    stage('Deploy') {
      parallel {
        stage('DeployDev') {
          steps {
            sh '''sudo docker rm -f $(sudo docker ps -aq)||true
sudo docker run -d -p 7777:80 --name devcon1 cpdevopstrng/srepo1:latest'''
          }
        }

        stage('DeployQA') {
          steps {
            sh '''sudo docker rm -f $(sudo docker ps -aq)||true
sudo docker run -d -p 9999:80 --name qacon1 cpdevopstrng/srepo1:latest'''
          }
        }

      }
    }

  }
}