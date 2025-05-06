pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git(url: 'https://github.com/sankalpmax/EV-Car-Application-CICD.git', branch: 'main')
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build('my-ev-application:01')
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    sh 'docker run -d -p 3000:3000 --name tesla my-ev-application:01'
                }
            }
        }
    }
}

