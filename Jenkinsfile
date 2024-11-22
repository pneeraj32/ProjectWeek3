pipeline {
    agent any
    stages {
        stage('Init') {
          steps {
            script {
              if(env.GIT_BRANCH == origin/main) {
                sh '''
                kubectl create ns prod || echo '-----Prod Namespace already exists-----'
                '''
              } else if(env.GIT_BRANCH == origin/dev) {
                sh '''
                kubectl create ns dev || echo '------Dev Namespace already exists-----'
                '''
              } else {
                sh'echo "Unknown branch"'
              }
            }
          }
        }
        stage('Building image') {
          steps {
            script{
              if(env.GIT_BRANCH == origin/main) {
                sh '''
                docker build -t neeraj870/project3:latest -t neeraj870/project3:v${BUILD_NUMBER} .
                '''
              } else if(env.GIT_BRANCH == origin/dev) {
                sh '''
                docker build -t neeraj870/project3_dev:latest -t neeraj870/project3_dev:v${BUILD_NUMBER} .
                '''
              } else {
                sh'echo"Unknown branch"'
              }
            }
          }
        }  
        stage('Deploy') {
            steps {
              script{
                if(env.GIT_BRANCH == origin/main) {
                  sh '''
                  kubectl apply -f ./kubernetes -n prod
                  kubectl set image deployment/node-deployment node-container=neeraj970/project3:v${BUILD_NUMBER} .
                  '''
                } else if(env.GIT_BRANCH == origin/dev) {
                  sh '''
                  kubectl apply -f ./kubernetes -n dev
                  kubectl set image deployment/node-deployment node-container=neeraj970/project3_dev:v${BUILD_NUMBER} .
                  '''
                } else {
                  sh'echo"Unknown branch"'
                }
             }
          }
        }
    }
}