pipeline {
    agent any
    stages {

        stage('Clone Repo') {
          steps {
            sh 'rm -rf dockertest1'
            sh 'git clone https://github.com/opntxt/dockertest1.git'
            }
        }

        stage('Build Docker Image') {
          steps {
            sh 'cd /home/jenkins/workspace/pipeline2/dockertest1'
            sh 'cp  /home/jenkins/workspace/pipeline2/dockertest1/* /home/jenkins/workspace/pipeline2'
            sh 'docker build -t sreeharshav/pipelinetestprod:${BUILD_NUMBER} .'
            }
        }

        stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push opntxt/pipelinetestprod:${BUILD_NUMBER}'
           }
        }

        stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://192.168.1.154:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://192.168.1.154:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 9000:80 opntxt/pipelinetestprod:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl http://192.168.1.154:9000'
          }
        }

    }
}
