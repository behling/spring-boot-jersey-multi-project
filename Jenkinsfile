pipeline {
    agent any

    triggers { 
        pollSCM('* * * * *') 
    }

    stages {
        stage('Build') {
            agent { 
                docker { image 'java:openjdk-8-jdk' } 
            } 
            steps { 
                sh 'ls'
                sh 'pwd'
                sh './gradlew clean build'
                sh 'ls -alF'
                /*
                dir('service-jersey-app/build/distributions/'){
                    stash includes: '*.zip', name: 'artefato'
                }  */
            }  
        }

        stage('Deploy PROD') {
            steps {
                timeout(time:1, unit:'DAYS') {
                    input message: 'Are you sure?'
                } 
                echo 'Deploy PROD'                
             /*   sshagent (credentials: ['servidor-treinamento']) {            
                     sh 'scp -o StrictHostKeyChecking=no service-jersey-app-1.1.zip ubuntu@172.31.9.116:~'
                     sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.9.116 "unzip service-jersey-app-1.1.zip"'
                } */
            }
        }
    }
}
