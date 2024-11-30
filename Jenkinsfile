pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQube'  // Nom de votre configuration SonarQube dans Jenkins
        MAVEN_HOME = '/usr/bin/mvn'  // Définir le chemin Maven, ajustez selon votre configuration
        NEXUS_URL = 'http://192.168.33.10:8081'  // URL de votre Nexus
    }

    stages {
        stage('Checkout') {
            steps {
                // Récupérer le code depuis GitHub
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Construire avec Maven
                sh "'${MAVEN_HOME}/bin/mvn' clean install -DskipTests"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Lancer l'analyse de SonarQube
                script {
                    // Assurez-vous d'avoir configuré SonarQube dans Jenkins
                    withSonarQubeEnv(SONARQUBE) {
                        sh "'${MAVEN_HOME}/bin/mvn' sonar:sonar"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                // Exécuter les tests JUnit avec Maven
                sh "'${MAVEN_HOME}/bin/mvn' test"
            }
        }

        stage('Publish to Nexus') {
            steps {
                script {
                    // Déployer les artefacts vers Nexus
                    sh "'${MAVEN_HOME}/bin/mvn' deploy -DaltDeploymentRepository=nexus::default::${'http://192.168.33.10:8081'}"
                }
            }
        }
    }

    post {
        always {
            // Action post-build, par exemple notifier en cas d'échec ou succès
            echo "Pipeline finished"
        }
    }
}
