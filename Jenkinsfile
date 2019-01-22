pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'test'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
  }
}