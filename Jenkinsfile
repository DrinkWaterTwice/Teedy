pipeline {
    agent any
    stages {
        stage('Package') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Building image') {
            steps {
                sh 'docker build -t teedy-image .' 
            }
        }
        stage('upload image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCredentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker tag teedy-image DrinkWaterTwice/my-image'
                    sh 'docker push DrinkWaterTwice/my-image'
                }
            }
        }
        stage('run container') {
            steps {
                sh 'docker run -d --name teedy-container DrinkWaterTwice/teedy-image'
            }
        }
    }
}