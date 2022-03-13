pipeline {
    agent any
    environment {
        AWS_URL = $aws_url
    }

    stages {
        stage('Build') {
            steps {
               echo "Building..."
               sh """
                cd simple_webserver
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${env.AWS_URL}
                docker build -t flask-app-yuval .
                docker tag flask-app-yuval:latest 352708296901.dkr.ecr.us-east-1.amazonaws.com/flask-app-yuval:latest
                docker push 352708296901.dkr.ecr.us-east-1.amazonaws.com/flask-app-yuval:latest
               """
            }
        }
        stage('Test') {
            steps {
                 echo "Testing..."
            }
        }
        stage('Deploy') {
            steps {
                 echo "Deploying..."
            }
        }
    }
}