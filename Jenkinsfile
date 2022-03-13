pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
               echo "building..."
               sh """
                cd simple_webserver
                docker build -t flask-app-yuval .
                docker tag flask-app-yuval:latest 352708296901.dkr.ecr.us-east-1.amazonaws.com/flask-app-yuval:latest
                docker push 352708296901.dkr.ecr.us-east-1.amazonaws.com/flask-app-yuval:latest
               """
            }
        }
        stage('Test') {
            steps {
                 echo "testing..."
            }
        }
        stage('Deploy') {
            steps {
                 echo "deploying..."
            }
        }
    }
}