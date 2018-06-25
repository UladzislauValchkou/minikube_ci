pipeline {
    agent any
    stages {
      stage('PR-tests') {
        when {
          branch "PR-*"
        }
        steps {
          echo 'stage1'
          println env.BRANCH_NAME
        }
      }
      stage('docker-build') {
        when {
          anyOf {
            branch "feature/*"
            branch 'dev'
          } 
        }
        steps {
          echo 'stage2'
        }
      }
      stage('kuber-namespace') {
        when {
          anyOf {
            branch "feature/*"
            branch 'dev'
          }
        } 
        steps {
          echo 'stage3'
        }
      }
      stage('deploy-kuber') {
        when {
          anyOf {
            branch "feature/*"
            branch 'dev'
          }
        }
        steps {
          echo 'stage4'
        }
      }
    }
} 
