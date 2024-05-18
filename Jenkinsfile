pipeline{
    agent any
    
    tools {
        jdk 'jdk17'
        maven 'Maven3'
    }
    environment {
        APP_NAME = "Maven-App"
        RELEASE = "1.0.0"
        // DOCKER_REGISTRY = "reg.tlandino.net"
        // DOCKER_USER = "docker"
        // DOCKER_PASS = "tetherland"
        IMAGE_NAME = "${DOCKER_REGISTRY}/${APP_NAME}:${RELEASE}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages{
        stage("Cleanup Workspace") {
            steps {
                script {
                    cleanWs()
                }
            }
        }
        stage("checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github' , url: 'https://github.com/reza-sadrinia/complete-prodcution-e2e-pipeline.git'
            }
        }
        stage("build") {
            steps {
                sh "mvn clean package"
            }
        }
        stage("test") {
            steps {
                sh "mvn test"
            }
        }
        stage("sonar scan") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage("Build and push"){
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', url: 'reg.tlandino.net') {
                        sh 'docker build -t $IMAGE_NAME .'
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }
    }
}
