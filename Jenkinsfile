pipeline {
    agent any
    stages {
        stage("verify tooling") {
            steps {
                sh '''
                    docker version
                    docker info
                    docker-compose version
                '''
            }
        }
        stage('Prune Docker data') {
            steps {
                sh 'docker system prune -af --volumes'
            }
        }
        stage('Start container') {
            steps {
                sh 'docker-compose up --build -d'
                sh 'docker-compose ps'
            }
        }
        stage('Run tests against the container') {
            steps {
                sh 'npm run docker-test'
            }
        }
    }
    post {
        always {
            sh 'docker-compose down --remove-orphans -v'
            sh 'docker-compose ps'
        }
    }
}
