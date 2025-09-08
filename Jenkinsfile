pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //sodsdsd that they can be downloaded later
            }
        }   
  stages {
      stage('Test') {
            steps {
              sh "mvn test"
            }
        }   
    }
}