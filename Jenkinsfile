
pipeline {
    agent any

    tools {
        jdk 'JDK-21'
        maven 'Maven-3.9.11'
    }

    environment {
        TOMCAT_URL = 'http://localhost:8080'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/safazaouali116-droid/e-commerce-1.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'tomcat-credentials',
                        usernameVariable: 'TOMCAT_USER',
                        passwordVariable: 'TOMCAT_PASSWORD'
                    )
                ]) {
                    sh '''
                        curl --fail --silent --show-error \
                        -u "$TOMCAT_USER:$TOMCAT_PASSWORD" \
                        --upload-file target/e-commerce.war \
                        "$TOMCAT_URL/manager/text/deploy?path=/e-commerce&update=true"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Déploiement réussi sur Tomcat'
        }
        failure {
            echo 'Échec de la pipeline'
        }
    }
}
