pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/NUCESFAST/scd-final-lab-exam-WajidAli05.git'
        FRONTEND_IMAGE = 'wajidali05/client:latest'
        AUTH_IMAGE = 'wajidali05/auth:latest'
        CLASSROOMS_IMAGE = 'wajidali05/classrooms:latest'
        EVENT_BUS_IMAGE = 'wajidali05/event-bus:latest'
        POST_IMAGE = 'wajidali05/post:latest'
        DOCKERHUB_CREDENTIALS = '0981'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git url: REPO_URL
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'cd client && npm install'
                        sh 'cd Auth && npm install'
                        sh 'cd Classrooms && npm install'
                        sh 'cd event-bus && npm install'
                        sh 'cd Post && npm install'
                    } else {
                        bat 'cd client && npm install'
                        bat 'cd Auth && npm install'
                        bat 'cd Classrooms && npm install'
                        bat 'cd event-bus && npm install'
                        bat 'cd Post && npm install'
                    }
                }
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    docker.build(FRONTEND_IMAGE, './client')
                    docker.build(AUTH_IMAGE, './Auth')
                    docker.build(CLASSROOMS_IMAGE, './Classrooms')
                    docker.build(EVENT_BUS_IMAGE, './event-bus')
                    docker.build(POST_IMAGE, './Post')
                }
            }
        }
        stage('Push Docker Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image(FRONTEND_IMAGE).push()
                        docker.image(AUTH_IMAGE).push()
                        docker.image(CLASSROOMS_IMAGE).push()
                        docker.image(EVENT_BUS_IMAGE).push()
                        docker.image(POST_IMAGE).push()
                    }
                }
            }
        }
        stage('Run Containers') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker network create mern-network'
                        docker.image(FRONTEND_IMAGE).run('-d --network mern-network -p 9811:3000')
                        docker.image(AUTH_IMAGE).run('-d --network mern-network -p 9812:4000')
                        docker.image(CLASSROOMS_IMAGE).run('-d --network mern-network -p 9813:4001')
                        docker.image(EVENT_BUS_IMAGE).run('-d --network mern-network -p 9814:4009')
                        docker.image(POST_IMAGE).run('-d --network mern-network -p 9815:4002')
                        sh 'docker run -d --network mern-network -p 9816:27017 --name mongodb mongo:latest'
                    } else {
                        bat 'docker network create mern-network'
                        bat "docker run -d --network mern-network -p 9811:3000 ${FRONTEND_IMAGE}"
                        bat "docker run -d --network mern-network -p 9812:4000 ${AUTH_IMAGE}"
                        bat "docker run -d --network mern-network -p 9813:4001 ${CLASSROOMS_IMAGE}"
                        bat "docker run -d --network mern-network -p 9814:4009 ${EVENT_BUS_IMAGE}"
                        bat "docker run -d --network mern-network -p 9815:4002 ${POST_IMAGE}"
                        bat 'docker run -d --network mern-network -p 9816:27017 --name mongodb mongo:latest'
                    }
                }
            }
        }
    }
    post {
        always {
            script {
                if (isUnix()) {
                    sh 'docker ps'
                } else {
                    bat 'docker ps'
                }
            }
        }
    }
}
