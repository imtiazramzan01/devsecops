pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later 
            }
        }   
      stage('Unit Tests') {
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
              script {
                docker.withRegistry('https://registry.hub.docker.com','DockerCred'){
                def customImage = docker.build("imtiaz01/numeric-app:${env.GIT_COMMIT}")
                customImage.push()}
              }
            }
        } 
    }
}