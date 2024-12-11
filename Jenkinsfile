pipeline {
    agent any
    // triggers {
    //     pollSCM('*/1 * * * *')
    // }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('rolandstech-dockerhub')
    }
    parameters {
        string(name: 'Greeting', defaultValue: 'Hello', description: 'How should I greet the world?')
    }
    stages {
        stage('Build-docker-image') {
            steps {
                buildDockerImage()
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

def buildDockerImage(){
    echo "Docker version..."
    echo "${$DOCKERHUB_CREDENTIALS_PSW}    and ${DOCKERHUB_CREDENTIALS_USR}"
    sh "docker --version"
    // echo "Building docker image..."
    // sh "docker build -t rolandstech/sample-book-app ."

    // echo "Login to Docker hub"
    // sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin "

    // echo "Pushing image to docker registry.."
    // sh "docker push rolandstech/sample-book-app"
} 


def deploy(String environment) {
    echo "Deployement treiggered on ${environment} env.."
    String lowercaseEnv = environment.toLowerCase();
    sh "docker compose stop sample-book-app-${lowercaseEnv}"
    sh "docker compose rm sample-book-app-${lowercaseEnv}"
    sh "docker compose up -d sample-book-app-${lowercaseEnv}"
}

def runApiTests(String environment) {
    echo "API tests treiggered on ${environment} env..."
}