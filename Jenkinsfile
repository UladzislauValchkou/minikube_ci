pipeline {
    agent any
    stages {
      stage('PR-tests') {
        when {
          branch "PR-*"
        }
        steps {
          checkPullRequest() 
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

def checkPullRequest() {
  def prNumber = env.BRANCH_NAME.split('-')[1]
  def buildNumber = env.BUILD_NUMBER
  if ((prNumber.toInteger() + buildNumber.toInteger())/2 == 0) {
    println 'tests passed'
  } else {
    error("Build failed because of bad PR number, bitch")
  }
} 
