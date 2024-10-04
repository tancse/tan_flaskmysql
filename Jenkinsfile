pipeline {
    agent any

    stages {
        stage('fetching github code') {
            steps {
                // using echo to print msg
                echo 'we are now fetching the code from git repo'
                // using git to download git repo
                Git branch:'main', url:'https://github.com/tancse/tan_flaskmysql.git'
                // using sh command to run some commands
                sh 'ls'
            }
        }
        stage('Checking Docker and compose status'){
            steps{
                echo 'checking docker status'
                sh 'docker version'
                echo 'checking docker-compose status'
                sh 'docker-compose version'
            }
        }
        // using compose and curl
        stage('compose for build and curl for app status test'){
            steps{
                echo 'running docke compose'
                sh 'docker-compose down'
                sh 'docker-compose up -d --build'
                sh 'docker-compose ps'
                sh 'curl -f http://localhost:3030'
            }
        }
        // using docker pipeline to build and push
        stage('building image and pushing it'){
            steps{
                echo 'uisng docker pipeline plugin to build and push image'
                script{
                    def imageName = "thananya/flaskappv1"
                    def imageTag = "appv1"
                    def tanCred = "bd03e4da-402b-4e38-86cc-06b227d2d51d"
                    // building image
                    docker.build(imageName + ":" + imageTag , "-f Dockerfile .")
                    //pushing image
                    docker.withRegistry('https://registry.hub.docker.com', tanCred){
                        docker.image(imageName + ":" + imageTag ).push()
                    }
                }
            }
        }
    }
}

