pipeline {
    agent any
    stages {
      stage('PR-tests') {
        when {
          branch "PR-*"
        }
        steps {
         println prNumber
         println env.BUILD_NUMBER
         println (prNumber.toInteger() + env.BUILD_NUMBER.toInteger())/2
        }
      }
      stage('docker-build') {
        when {
          not {
            branch "PR-*"
          } 
        }
        steps {
          echo 'stage2'
        }
      }
      stage('kuber-namespace') {
        when {
          not {
            branch "PR-*"
          }
        } 
        steps {
          echo 'stage3'
        }
      }
      stage('deploy-kuber') {
        when {
          not {
            branch "PR-*"
          }
        }
        steps {
          echo 'stage4'
        }
      }
    }
}

prNumber = env.BRANCH_NAME.split('-')[1]

def checkPullRequest() {
  prNumber = env.BRANCH_NAME.split('-')[1]
  if ((prNumber.toInteger() + env.BUILD_NUMBER.toInteger())/2 == 0) {
    println tests passed
  } else {
    error("Build failed because of bad PR number, bitch")
  }
} 
