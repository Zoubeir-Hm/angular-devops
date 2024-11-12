 pipeline {
    agent any

    environment {
        // SonarQube server URL and token
        SONAR_HOST_URL = 'http://localhost:9000'
        SONAR_TOKEN = 'sqp_b1c84db3309bede3505a8c2409989ba8df7faf4d'
        // Adding the SonarScanner path
        PATH = "/opt/sonar-scanner/bin:$PATH"
    }

    stages {
        stage("Build Docker Imag") {
            steps {
                script {
                    sh 'docker build -t your-angular-app1 .'
                }
            }
        }

      stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner'
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=Angular \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=squ_03720bec86e889a86de054b32a602e106b2129ba \
                            -Dsonar.sources=./app \
                            "
                        """
                    }
                }
            }
        }    

       

        stage("Deploy Docker Container") {
            steps {
                script {
                    sh 'docker stop your-angular-app1-container || true'
                    sh 'docker rm your-angular-app1-container || true'
                    sh 'docker run -d -p 8087:80 --name your-angular-app1-container your-angular-app1'
                }
            }
        }
    }
}
