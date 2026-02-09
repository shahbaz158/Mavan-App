pipeline {
    agent any

    stages {
        stage('git clone') {
            steps {
                git branch: 'main', credentialsId: 'Git-credential', url: 'https://github.com/shahbaz158/Mavan-App.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker images') {
            steps {
                sh 'docker build -t shahbazhcl/my-maven-app .'
            }
        }
        stage('Docker push') {
          steps {
         withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
        sh '''
          echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
          docker build -t shahbazhcl/my-maven-app:latest .
          docker push shahbazhcl/my-maven-app:latest
          '''

          }
        }
    }
        stage('Deploy images') {
            steps{
               sh "kubectl apply -f deployment.yaml"

               sh "kubectl rollout restart deployment javawebappdeployment"

               sh "kubectl rollout status deployment javawebappdeployment"
            }
        }
    }
}
