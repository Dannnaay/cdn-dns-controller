pipeline {
    enviroment {
        repository = "https://github.com/Dannnaay/cdn-dns-controller.git"
        repositoryCredentials = "Dannnaay"
        imageName = "cdn-dns-controller"
    }
    agent any
    options {
        timeout(time: 1, unit: 'HOURS')
    }
    stages {
        stage('git clone') {
            steps {
                bat 'git clone https://github.com/Dannnaay/cdn-dns-controller.git'
            }
        }
        stage('tests') {
            steps {
                bat 'python3 -m pytest tests/'
            }
        }
        stage('create docker image') {
            steps {
                bat 'docker build -t cdn-dns-controller -f docker/Dockerfile .'
            }
        }
        stage('run docker') {
            steps {
                bat 'docker run -it -d -p 5000:5000 --rm --name cdn-dns-controller cdn-dns-controller'
            }
        }
        stage('sanity test') {
            steps {
                bat 'curl localhost:5000/'
            }
        }
        stage('cleanup') {
            steps {
                bat 'docker rm -f cdn-dns-controller'
            }
        }
    }
}