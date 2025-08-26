pipeline {
    agent {
        docker {
            image 'docker:28.0'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Thanutham-T/jenkins-demo-app', credentialsId: '885836b7-3cb3-4c86-894b-68009494abad'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build --no-cache -t jenkins-demo-app:latest .'
            }
        }
        stage('Run Container') {
            steps {
                sh 'docker rm -f demo-app || true'
                sh 'docker run -d -p 8081:8081 --name demo-app jenkins-demo-app:latest || true'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                sh 'docker exec demo-app pytest || true'
            }
        }
    }
}
