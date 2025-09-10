pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //sodsdsd that they can be downloaded later
            }
        }   
      stage('Test') {
            steps {
              sh "mvn test"
            }
            post{
              junit 'target/surefire-reports/*.xml'
              jacoco execPattern: 'target/jacoco.exec'
            }
        }   
    }
      stage('Docker Buold and Push'){
        steps {
          sh 'printenv'
          sh 'docker build -t aplicacion/app:""$GIT_COMMIT"".'
          sh 'docker push aplicacion/app:""$GIT_COMMIT""'
        }
      }


}