pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('rolandstech-dockerhub') // This fetches the credentials from Jenkins credentials store
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
    }
    stages {
        stage('Build-docker-image') {
            steps {
                script {
                    buildDockerImage()
                }
            }
        }
        stage('deploy-dev') {
            steps {
                deploy("DEV")
            }
        }
        stage('test-dev') {
            steps {
                runApiTests("DEV")
            }
        }
        stage('deploy-stg') {
            steps {
                deploy("STG")
            }
        }
        stage('test-stg') {
            steps {
                runApiTests("STG")
            }
        }
        stage('deploy-prd') {
            steps {
                deploy("PRD")
            }
        }
        stage('test-prd') {
            steps {
                runApiTests("PRD")
            }
        }
    }
}

def buildDockerImage() {
    echo "Docker version..."
    
    // Securely pass the Docker credentials into the Docker login command using 'withCredentials'
    withCredentials([usernamePassword(credentialsId: 'rolandstech-dockerhub', usernameVariable: 'DOCKERHUB_USR', passwordVariable: 'DOCKERHUB_PSW')]) {
        echo "Logging in to DockerHub..."
        sh "docker login -u $DOCKERHUB_USR -p $DOCKERHUB_PSW"
    }
    
    // Build Docker image
    echo "Building docker image..."
    powershell "docker build -t rolandstech/sample-book-app ."
    
    // Push the image to DockerHub
    echo "Pushing image to DockerHub..."
    powershell "docker push rolandstech/sample-book-app"
}

def deploy(String environment) {
    echo "Deployment triggered on ${environment} environment..."
    String lowercaseEnv = environment.toLowerCase()
    powershell "docker compose stop sample-book-app-${lowercaseEnv}"
    powershell "docker compose rm sample-book-app-${lowercaseEnv}"
    powershell "docker compose up -d sample-book-app-${lowercaseEnv}"
}

def runApiTests(String environment) {
    echo "Running API tests in the ${environment} environment..."
    // Add your API test logic here
}
