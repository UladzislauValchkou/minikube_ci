pipeline {
    environment {
      fixedBranch = fixBranchName()
    }
    agent none 
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
        agent {
          kubernetes {
            label 'docker'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: docker
spec:
  containers:
  - name: docker
    image: docker
    volumeMounts:
      - name: docker-sock
        mountPath: /var/run/docker.sock
    command:
    - cat
    tty: true
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock        
"""
          }
        }
        steps {
          container('docker') {
            sh 'docker build -t test-nginx:${fixedBranch}-${BUILD_NUMBER} .'            
          }
        }
      }     
      stage('kuber-namespace') {
        when {
          not {
            branch "PR-*"
          }
        }
        agent {
          kubernetes {
            label 'kubectl'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: kubectl
spec:
  containers:
  - name: kubectl
    image: lachlanevenson/k8s-kubectl
    command:
    - cat
    tty: true
"""
          }
        }
        steps {
          container('kubectl') {
            try {
              sh 'kubectl get namespace | grep ${fixedBranch}'
            } catch (err) { 
              sh 'kubectl create namespace ${fixedBranch}'
            }   
          }
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

def fixBranchName() {
  def valid_name = env.BRANCH_NAME.replaceAll('/', '-')
   return valid_name 
}
