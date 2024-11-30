pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://192.168.33.10:9000'  // URL de votre serveur SonarQube
        SONAR_TOKEN = credentials('GUBGMHVpwcTsPFvNNVdG5DKtpv6UwksAfzw5aULcGzgG')    // Nom du credential que vous avez créé dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/ghadaboukhari/station-ski.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Exécute l'analyse SonarQube
                    sh 'mvn sonar:sonar -Dsonar.projectKey=station-ski -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_TOKEN}'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Le pipeline a été exécuté avec succès!'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
