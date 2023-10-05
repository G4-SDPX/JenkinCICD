pipeline {
    agent any
        stages {
        stage('Install Environment') {
            agent {
                label 'test'
            }
            steps {
                sh 'python3 -m venv env'
            }
        }
        stage('Activate Environment && Install Library') {
            agent {
                label 'test'
            }
            steps {
                sh '''#!/bin/bash
                 source env/bin/activate && pip install -r ./requirements.txt
                '''
            }
        }
        stage('Run Unittest') {
            agent {
                label 'test'
            }
            steps {
                sh '''#!/bin/bash
                 source env/bin/activate && python3 ./unit_test.py
                '''
            }
        }
        stage('Run Robot') {
            agent {
                label 'test'
            }
            steps {
                echo 'Create Container'
                sh 'docker-compose -f ./compose.yaml up -d --build'
                echo 'Runing Robot'
                sh 'robot ./test-calculate.robot'
            }
        }
        stage('Push Registry') {
            agent {
                label 'test'
            }
            steps {
                echo 'Logging'
                sh 'docker login registry.gitlab.com'
                echo 'Build Images'
                sh 'cd app && docker build -t registry.gitlab.com/sasihan/jenkinscicdtesting .'
                echo 'Push Images'
                sh 'docker push registry.gitlab.com/sasihan/jenkinscicdtesting'
            }
        }
        stage('Clean Workspace') {
            agent {
                label 'test'
            }
            steps {
                echo 'DownTime'
                sh 'docker-compose -f ./compose.yaml down'
                sh 'docker system prune -a -f'
            }
        }
        stage('Stop and Remove Docker Container') {
            agent {
                label 'pre-prod'
            }
            steps {
                echo 'Show Docker running'
                sh 'docker ps'
                echo 'Stop Docker all'
                sh 'docker stop `docker ps -q` || true'
                echo 'Delete Docker all'
                sh 'docker rm `docker ps -aq` || true'
            }
        }
        stage('Run Docker Container') {
            agent {
                label 'pre-prod'
            }
            steps {
                echo 'Pull Image from Gitlab'
                sh 'docker pull registry.gitlab.com/sasihan/jenkinscicdtesting'
                echo 'Run Contrainer'
                sh 'docker run -d -p 5000:5000 registry.gitlab.com/sasihan/jenkinscicdtesting'
            }
        }
    }
    
}
// hangster10761@gmail.com
// glpat-nL2ieK98aHb4pz6M6P8c