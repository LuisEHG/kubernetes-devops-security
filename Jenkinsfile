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
                withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                    sh 'printenv'
                    sh "docker build -t luiseduhg/app:${GIT_COMMIT} ."
                    sh "docker push luiseduhg/app:${GIT_COMMIT}"
                }
            }
        }
        
       stage('Kubernetes Deployment - dev') {
    steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
            // Verificar conexión
            sh "kubectl cluster-info"
            sh "kubectl get nodes"
            
            // Aplicar deploymentsss
            sh "sed -i 's#replace#luiseduhg/app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
            sh "kubectl apply -f k8s_deployment_service.yaml"
        }
    }
}
    }
}