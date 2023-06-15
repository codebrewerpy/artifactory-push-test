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
          sh 'curl -fL https://getcli.jfrog.io | sh'
          sh './jfrog rt config --url=http://172.17.0.3:8082/artifactory/ --access-token=${ARTIFACTORY_ACCESS_TOKEN}'
          sh './jfrog rt u target/demo-0.0.1-SNAPSHOT.jar test_repo/'
        }
      }
    }
  }
}
