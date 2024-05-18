pipeline{
    agent any
    
    tools {
        jdk 'jdk17'
        maven 'Maven3'
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
                git branche: 'main' credentialsId: 'github' , url: 'https://github.com/reza-sadrinia/complete-prodcution-e2e-pipeline.git'
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
    }
}
