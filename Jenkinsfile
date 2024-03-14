pipeline {
    agent any 
    environment {
        PATH = "/usr/bin:$PATH"
        tag = "1.0"
        dockerHubUser = "syerukon"
        containerName = "insure-me"
        httpPort = "8081"
    }
    stages {
        stage("code clone") {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sindhuyerukon/asi-insurance.git']])
            }
        }
        stage("Maven build") {
            steps {
                sh "mvn clean install -DskipTests"
            }
        }
        stage("Build Docker Image") {
            steps {
                sh "docker build -t ${dockerHubUser}/insure-me:${tag} ."
            }
        }
        stage("push image to dockerhub") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {
                    sh "docker login -u $dockerUser -p $dockerPassword"
                    sh "docker push $dockerUser/$containerName:$tag"
                }
            }
        }
    }
}

