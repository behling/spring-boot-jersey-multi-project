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
                dir('service-jersey-app/build/distributions/'){
                    stash includes: '*.zip', name: 'artefato'
                }
            }  
        }

        stage('Deploy PROD') {
            steps {
                timeout(time:1, unit:'DAYS') {
                    input message: 'Are you sure?'
                }
                unstash 'artefato'
                echo 'Deploy PROD'                
                sshagent (credentials: ['servidor-treinamento']) {
                     sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.9.116 yum install -y unzip'
                     sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.9.116 mkdir -p Cleiton'
                     sh 'scp -o StrictHostKeyChecking=no service-jersey-app-1.1.zip ubuntu@172.31.9.116:~/Cleiton/'
                     sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.9.116 "cd Cleiton && unzip service-jersey-app-1.1.zip"'
                } 
            }
        }
    }
}
