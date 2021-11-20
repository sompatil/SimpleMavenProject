pipeline {
    agent any

    stages {
        stage('Build Java Application') {
            steps {
                sh 'sudo mvn clean install package'
            }
        }
        stage('Docker Image Build For My Java Application') {
            steps {
                sh 'sudo docker build -t java-app .'
            }
        }
        stage('Tag Image with Repository Name') {
            steps {
                sh 'sudo  docker tag java-app dab8106/java-app'
            }
        }
        
        stage('DockerLogin') {
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'pass', usernameVariable: 'user')]) {
                sh 'sudo docker login --username dab8106 --password-stdin $pass'
                }
            }
        }
        
        stage('Pushing the image') {
            steps {
                sh 'sudo docker push dab8106/java-app'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s.yml'
            }
        }
    }
}
