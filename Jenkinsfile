pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "sankalparava/my-ev-application:01"
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/sankalpmax/EV-Car-Application-CICD.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-ev-application:01 .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name my-ev-app-container my-ev-application:01'
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        sh 'docker tag my-ev-application:01 sankalparava/my-ev-application:01'
                        sh 'docker push sankalparava/my-ev-application:01'
                    }
                }
            }
        }

        stage('OWASP Dependency-Check') {
            steps {
        	              // Download and extract OWASP Dependency-Check
       		 sh '''
           	 DEPENDENCY_CHECK_VERSION="8.4.0"
           	 wget -q https://github.com/jeremylong/DependencyCheck/releases/download/v${DEPENDENCY_CHECK_VERSION}/dependency-check-${DEPENDENCY_CHECK_VERSION}-release.zip
         	 unzip -q dependency-check-${DEPENDENCY_CHECK_VERSION}-release.zip
       		 '''

        	// Run the dependency-check scan
       		 sh '''
           	 ./dependency-check/bin/dependency-check.sh \
           	 --project "MyApp" \
           	 --scan . \
            	--format HTML \
            	--out dependency-check-report
        	'''
			 }
       		 }
   	 } // End of stages
} // End of pipeline

