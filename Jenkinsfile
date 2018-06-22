pipeline {
    agent any
    stages {
      stage('PR-tests') {
        steps {
          echo 'stage1'
          println ${env.BRANCH_NAME}
        }
      }
      stage('docker-build') {
        steps {
          echo 'stage2'
        }
      }
      stage('kuber-namespace') {
        steps {
          echo 'stage3'
        }
      }
      stage('deploy-kuber') {
        steps {
          echo 'stage4'
        }
      }
    }
} 
