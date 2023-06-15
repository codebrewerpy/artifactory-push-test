pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    CI = true
    ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
  }
  stages {
    stage('Build') {
      steps {
        sh './mvnw clean install'
      }
    }
    stage('Upload to Artifactory') {
      steps {
        script {
          sh 'pwd'
          sh 'ls target' // Verify that the artifact is present in the target directory
          sh './jfrog rt u target/demo-0.0.1-SNAPSHOT.jar test_repo/'
        }
      }
    }
  }
}
