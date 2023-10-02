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
                echo 'Cloning Robots'
                dir('./robot/') {
                    git branch: 'main', url: 'https://github.com/CE-SDPX/simple-api-robot.git'
                }
                echo 'Runing Robot'
                sh 'cd ./robot && robot ./test-calculate.robot'
            }
        }
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
