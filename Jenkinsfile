pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Generate Javadoc') {
            steps {
                sh 'mvn javadoc:javadoc'
            }
        }
        stage('PMD') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/site/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/site/apidocs/**', fingerprint: true
            archiveArtifacts artifacts: '**/target/pmd.xml', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
        }
    }
}