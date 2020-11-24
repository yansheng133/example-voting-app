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
  }
  post{
    always{
      archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      echo 'Building multibranch pipeline for worker is completed:)'
    }
  }
}
