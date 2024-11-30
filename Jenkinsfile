pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9000'  // URL de votre SonarQube
        SONAR_TOKEN = 'your-sonar-token'          // Remplacez par votre token SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupère le code depuis GitHub
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/Ghada-boukhari/-station-ski.git'
            }
        }

        stage('Build') {
            steps {
                // Compile et installe avec Maven
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Lance l'analyse SonarQube avec Maven
                script {
                    sh 'mvn sonar:sonar -Dsonar.host.url=${SONAR_HOST_URL} -Dsonar.login=${SONAR_TOKEN}'
                }
            }
        }

        stage('Push to Nexus') {
            steps {
                // Déploie le fichier .jar sur Nexus
                script {
                    sh 'mvn deploy:deploy-file -Dfile=target/your-artifact.jar -DrepositoryId=nexus-releases -Durl=http://localhost:8081/repository/maven-releases/ -DgroupId=com.example -DartifactId=station-ski -Dversion=1.0.0 -Dpackaging=jar'
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
