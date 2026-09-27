pipeline {

    agent any

    options {
        disableConcurrentBuilds()
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'Testing website files...'

                sh '''
                    test -f index.html
                    echo "index.html found successfully"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying website to Nginx...'

                sh '''
                    rsync -av --delete \
                    --exclude='.git/' \
                    --exclude='Jenkinsfile' \
                    ./ /var/www/html/
                '''
            }
        }

        stage('Verify') {
            steps {
                echo 'Verifying Nginx website...'

                sh '''
                    curl -I http://localhost
                '''
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESSFUL - Website deployed to Nginx'
        }

        failure {
            echo 'BUILD FAILED - Check Jenkins Console Output'
        }
    }
}
