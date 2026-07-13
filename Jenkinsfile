pipeline {
    agent {
        label 'worker'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/harshith00000/Ansible.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t rapido-webapp:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                    docker stop rapido-container || true
                    docker rm rapido-container || true
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker run -d \
                    --name rapido-container \
                    -p 8080:80 \
                    rapido-webapp:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Application deployed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
