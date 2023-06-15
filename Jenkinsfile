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
          sh 'docker pull releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0'
          sh 'docker inspect -f . releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0'
          sh 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0 jfrog -v'
          sh 'docker run --rm releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0 jfrog rt upload --url http://172.17.0.3:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/demo-0.0.1-SNAPSHOT.jar test_repo/'
        }
      }
    }
  }
}
