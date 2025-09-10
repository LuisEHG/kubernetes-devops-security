pipeline {
    agent any

    stages {
        stage('Build Artifact') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar' // Para que puedan ser descargados después
            }
        }   
        
        stage('Test') {
            steps {
                sh "mvn test"
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco execPattern: 'target/jacoco.exec'
                }
            }
        }   
        
        stage('Docker Build and Push') {
            steps {
                sh 'printenv'
                sh "docker build -t luiseduhg/app:${GIT_COMMIT} ."
                sh "docker push luiseduhg/app:${GIT_COMMIT}"
            }
        }
    }
    }
