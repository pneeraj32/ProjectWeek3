pipeline {
    agent any
    stages {
        stage('Building image') {
          steps {
            docker build -t $DOCKER_IMAGE .
          }
        }      

        stage('Build step') {
            steps {
                sh "sh setup.sh"
       }
     }
   }
}
