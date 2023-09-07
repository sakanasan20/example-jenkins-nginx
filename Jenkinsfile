pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }

    stages {
        stage('Fetch') {
            steps {
                git (
                    url: 'https://github.com/sakanasan20/example-docker-nginx.git',
                    branch: 'main'
                )
            }
        }
        
        stage('Copy Files') {
            steps {
                sh 'mkdir -p /usr/share/docker/nginx'
                sh 'cp -r --remove-destination ./* /usr/share/docker/nginx'
            }
        }
        
        stage('Build') {
            steps {
                dir('/usr/share/docker/nginx') {
                    sh 'docker compose build'
                }
            }
        }
        
        stage('Login') {
            steps {
                dir('/usr/share/docker/nginx') {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }
        
        stage('Push') {
            steps {
                dir('/usr/share/docker/nginx') {
                    sh 'docker push nickchen20/nginx'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                dir('/usr/share/docker/nginx') {
                    sh 'docker stack deploy -c docker-compose.yml nginx'
                }
            }
        }
    }
}
