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
  int prNumber = env.BRANCH_NAME.split('-')[-1] as Integer
  int buildNumber = env.BUILD_NUMBER as Integer
  int res = (prNumber + buildNumber) as Integer 
  println res
  if (res % 2 == 0) {
    println 'tests passed'
  } else {
    error("Build failed because of bad PR number, bitch")
  }
} 
