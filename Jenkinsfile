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
                sh 'docker compose -f ./compose.yaml up -d --build'
                echo 'Runing Robot'
                sh 'robot ./test-calculate.robot'
            }
        }
        stage('Push Images to registry') {
            agent {
                label 'test'
            }
            steps {
                echo 'Logging'
                sh 'docker login registry.gitlab.com'
                echo 'Build Images'
                sh 'cd app && docker build -t registry.gitlab.com/SasiHan/jenkinscicdtesting .'
                echo 'Push Images'
                sh 'docker push registry.gitlab.com/SasiHan/jenkinscicdtesting'
            }
        // }
        stage('Clean Workspace') {
            agent {
                label 'test'
            }
            steps {
                echo 'DownTime'
                sh 'docker compose -f ./compose.yaml down'
                sh 'docker system prune -a -f'
            }
        }
    }
}

// hangster10761@gmail.com
// glpat-nL2ieK98aHb4pz6M6P8c