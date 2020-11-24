pipeline{
  agent any
  
  tools{
    maven 'maven 3.6.1'
  }
  
  stages{
    stage('build'){
      steps{
        echo 'compiling worker app'
        dir('worker'){
          sh 'mvn compile'
        }
      }
    }
    stage('test'){
      steps{
        echo 'running unit test on worker app'
        dir('worker'){
          sh 'mvn clean test'
        }
      }
    }
    stage('package'){
      steps{
        echo 'packaging worker app'
        dir('worker'){
          sh 'mvn package -DskipTests'
        }
     }
    }
    stage('Sonarqube'){
      agent any
      environment{
        sonarpath = tool 'SonarQubeScanner'
      }   
      steps{
        echo 'Running Sonarqube Analysis...'
          withSonarQubeEnv('sonar'){
            sh "${sonarpath}/bin/sonar-scanner -Dproject.setting=sonar-project.properties"
          }
      }
    }
  }
  post{
    always{
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      echo 'Building multibranch pipeline for worker is completed:)'
    }
    failure{
      slackSend(channel: "#jenkins-test", message: "Build Fail - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}>)")
    }
    success{
      slackSend(channel: "#jenkins-test", message: "Build Success - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}>)")
    }
  }
}
