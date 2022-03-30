pipeline {
    agent any
    environment {
        AWS_URL = '352708296901.dkr.ecr.us-east-1.amazonaws.com'
        PATH = "./venv/bin:$PATH" // Add pip virtual-environment's path to PATH

    }
      stages {
        stage ('Create venv') {
            steps {
                sh '''
                # mkdir ./venv
                python3 -m venv ./venv
                source ./venv/bin/activate
                '''
            }
        }

    stages {
        stage('Artifactory Configurations'){
            steps{
                rtServer (
                    id: 'Artifactory-1',
                    url: 'http://my-artifactory-domain/artifactory',
                    // If you're using Credentials ID:
                    credentialsId: 'ccrreeddeennttiiaall',
                    // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
                    bypassProxy: true,
                    // Configure the connection timeout (in seconds).
                    // The default value (if not configured) is 300 seconds:
                    timeout: 300
            )

            }

        }

        stage('Build') {
             when {
                 branch 'dev'
             }
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
            post {
                 always {
                     // previous to version 2.0.0 you must provide parameters to this command (see below)!
                     jiraSendBuildInfo()
                 }
             }
        }
        stage('Test') {
            when { changeRequest() }
            steps {
                sh '''
                pip install -r simple_webserver/requirements.txt
                PYTHONPATH=. python3 -m pytest --junitxml results.xml simple_webserver/tests
                '''
                }
            }
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