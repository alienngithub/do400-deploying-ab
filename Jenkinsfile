pipeline {
    agent {
        label 'maven'
    }
    tools {
        maven "maven"
    }
   
    
    stages {
        
        
     
        stage ('Unit Tests') {
            steps {
                
                    sh "mvn verify"
                
            }
        }
        stage ('Build image') {
            environment { QUAY = credentials('QUAY_USER') }
                steps {
                    sh '''
                      mvn quarkus:add-extension \
                      -Dextensions="container-image-jib"
                      mvn quarkus:add-extension \
                      -Dextensions="kubernetes"
                      '''
                      sh '''
                        mvn package -DskipTests \
                        -Dquarkus.jib.base-jvm-image=quay.io/redhattraining/do400-java-alpine-openjdk11-jre:latest \
                        -Dquarkus.container-image.build=true \
                        -Dquarkus.container-image.registry=quay.io \
                        -Dquarkus.container-image.group=$QUAY_USR \
                        -Dquarkus.container-image.name=do400-deploying-ab \
                        -Dquarkus.container-image.username=$QUAY_USR \
                        -Dquarkus.container-image.password="$QUAY_PSW" \
                        -Dquarkus.container-image.tag=build-${BUILD_NUMBER} \
                        -Dquarkus.container-image.additional-tags=latest \
                        -Dquarkus.container-image.push=true
                        '''

                }

        }
        stage('Deploy to TEST') {
            when { not { branch "main" } }
            steps {
                sh '''
oc set image deployment home-automation \
home-automation=quay.io/${QUAY_USR}/do400-deploying-ab:build-${BUILD_NUMBER} \
-n xudlbs-deploying-lab-test --record
                '''
            }
        }
        stage('Deploy to PROD') {
            when { branch "main" }
            steps {
                sh '''
oc set image deployment home-automation \
home-automation=quay.io/${QUAY_USR}/do400-deploying-ab:build-${BUILD_NUMBER} \
-n xudlbs-deploying-lab-prod --record
                '''
            }
        }
    }
}