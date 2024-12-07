pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
    }
    stages {
        stage('Build-docker-image') {
            steps {
                echo "Building docker image"
            }
        }
        stage('deploy-dev') {
            steps {
                echo "Deployement treiggered on DEV env.."
            }
        }
        stage('test-dev') {
            steps {
                echo "API tests triggered on DEV env.."
            }
        }
        stage('deploy-prd') {
            steps {
                echo "Deployement treiggered on PRD env.."
            }
        }
        stage('test-prd') {
            steps {
                echo "API tests treiggered on PRD env.."
            }
        }
    }
}