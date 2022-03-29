pipeline {
    agent any
    environment {
        AWS_URL = '352708296901.dkr.ecr.us-east-1a.amazonaws.com'
    }

    stages {
        stage('Build') {
            steps {
               echo "Building..."
               sh """
                cd simple_webserver
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${AWS_URL}
                docker build -t flask-app-yuval:${BUILD_NUMBER} .
                docker tag flask-app-yuval:${BUILD_NUMBER} 352708296901.dkr.ecr.us-east-1.amazonaws.com/flask-app-yuval:${BUILD_NUMBER}
                docker push 352708296901.dkr.ecr.us-east-1.amazonaws.com/flask-app-yuval:${BUILD_NUMBER}
               """
            }
        }
        stage('Test') {
            when{changeRequest target: 'dev'}
            steps {
                 echo "Testing..."
                 sh '''
                 pip3 install -r simple_webserver/requirements.txt
                 PYTHONPATH=. python3 -m pytest --junitxml results.xml simple_webserver/tests
                 '''

            }
              post {
                always {
                    junit(allowEmptyResults: true,testResults: 'simple_webserver/results.xml')
    }}
        }
        stage('Deploy') {
            steps {
                 echo "Deploying..."
            }
        }
    }
}