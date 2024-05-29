pipeline {
    agent {
        label 'maven'
    }
    tools {
        maven "maven"
    }
    
    stages {
        stage ('Install Dependencies') {
            steps {
                sh "echo id"
                }
            }
        }
     
        stage ('Unit Tests') {
            steps {
                
                    sh "mvn verify"
                
            }
        }
        stage ('Functional Tests') {
            steps {
               
                    sh "echo tak"
                
            }
        }
    }
}