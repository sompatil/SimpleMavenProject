pipeline {
    agent any

    stages {
        stage('Build Java Application') {
            steps {
                sh 'mvn clean install package'
            }
        }
        stage('Docker Image Build For My Java Application') {
            steps {
                sh 'docker build -t java-app .'
            }
        }
        stage('Tag Image with Repository Name') {
            steps {
                sh 'docker tag java-app dab8106/java-app'
            }
        }
        
        stage('DockerLogin') {
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'pass', usernameVariable: 'user')]) {
                sh 'docker login -u dab8106 -p $pass'
                }
            }
        }
        
        stage('Pushing the image') {
            steps {
                sh 'docker push dab8106/java-app'
            }
        }
        stage('Deploy') {
            steps {
                sh 'kubectl apply -f k8s.yml'
            }
        }
    }
}
