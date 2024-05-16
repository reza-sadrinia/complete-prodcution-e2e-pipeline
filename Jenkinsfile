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
    }
}
