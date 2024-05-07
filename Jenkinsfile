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
                    sh 'docker tag teedy-image drinkwatertwice/teedy:teedy-image'
                    sh 'docker push drinkwatertwice/teedy:teedy-image'
                }
            }
        }
        stage('run container') {
            steps {
                sh 'docker run -d -p 8081:8080 --name teedy-container1 drinkwatertwice/teedy:teedy-image'
                sh 'docker run -d -p 8082:8080 --name teedy-container2 drinkwatertwice/teedy:teedy-image'
                sh 'docker run -d -p 8083:8080 --name teedy-container3 drinkwatertwice/teedy:teedy-image'
            }
        }
    }
}