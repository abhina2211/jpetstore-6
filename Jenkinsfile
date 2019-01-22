pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'starting build'
        sh 'mvn clean install -Dlicense.skip=true'
      }
    }
    stage('Testing ') {
      parallel {
        stage('Sonar test') {
          steps {
            sh 'mvn sonar:sonar -Dsonar.host.url=http://54.255.175.71:8081 -Dlicense.skip=true'
          }
        }
        stage('Print tester credential') {
          steps {
            echo "The tester is ${tester}"
          }
        }
        stage('Print Build Number') {
          steps {
            echo "The build number is ${BUILD_ID}"
          }
        }
      }
    }
    stage('Jfrog push') {
      steps {
        echo 'Start Jfrog push'
        script {
          def server = Artifactory.server "artifactory"
          def buildInfo = Artifactory.newBuildInfo()
          def rtMaven = Artifactory.newMavenBuild()

          rtMaven.tool = 'maven'
          rtMaven.deployer server: server, releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'

          buildInfo = rtMaven.run pom: 'pom.xml', goals: "clean install -Dlicense.skip=true"
          buildInfo.env.capture = true
          buildInfo.name = 'jpetstore-6'
          server.publishBuildInfo buildInfo
        }

      }
    }
    stage('Deploy prompt') {
      steps {
        input 'Do you want to Deploy to production'
      }
    }
    stage('Deployment') {
      steps {
        echo 'Deployment Completed'
      }
    }
  }
  tools {
    maven 'maven'
  }
  environment {
    TESTER = 'Abhinav'
  }
}