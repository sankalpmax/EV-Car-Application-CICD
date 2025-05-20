pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/sankalpmax/EV-Car-Application-CICD.git'
            }
        }

        stage('Docker Build Image') {
            steps {
                sh 'docker build -t sankalparava/ev-car:02 .'
            }
        }

        stage('Docker Run Container') {
            steps {
                sh '''
                docker run -d -p 3000:3000 --name tesla sankalparava/ev-car:02
                '''
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        sh 'docker push sankalparava/ev-car:02'
                    }
                }
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                sh '''
                kubectl apply -f tesla-car-deployment.yaml
     		kubectl apply -f tesla-car-service.yaml           
                '''
            }
        }
    }
}
