pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7'
        jdk 'JDK11'
    }

    environment {
        // El nombre del servidor configurado en Jenkins > Configure System
        SONARQUBE_SERVER = 'Sonar 9.9.8'
        // Token configurado en Jenkins Credentials
        SONAR_TOKEN = credentials('sonar-token-id2')
    }

    stages {
        stage('Clonar') {
            steps {
                git 'https://github.com/DanielAL22/jenkins-sq-test.git'
            }
        }

        stage('Compilar') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Empaquetar') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Análisis SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh """
                        mvn sonar:sonar \
                        -Dsonar.projectKey=saludoapp \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completado correctamente"
        }
        failure {
            echo "💥 El pipeline falló"
        }
    }
}
