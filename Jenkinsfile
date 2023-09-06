pipeline {
    agent any

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
        
        stage('Deploy') {
            steps {
                dir('/usr/share/docker/nginx') {
                    sh 'docker stack deploy -c docker-compose.yml nginx'
                }
            }
        }
    }
}
