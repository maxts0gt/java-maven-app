#!/usr/bin/env groovy

library identifier: 'jenkins-shared-library@master', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/maxts0gt/java-maven-app.git',
     credentialsId: 'github-credentials'
     ]
)

pipeline {
    agent any
    
    tools {
        maven 'Maven'
    }
    
    environment {
        IMAGE_NAME = "maxts0gt/jenkins-demo-app:jma-1.0.0"
    }
    stages {
        stage("build app") {
            steps {
                script {
                    echo 'building application jar...'
                    buildJar()
                }
            }
        }
        stage("build image") {
            steps {
                script {
                    echo 'building docker image...'
                    buildImage(env.IMAGE_NAME)
                    dockerLogin()
                    dockerPush(env.IMAGE_NAME)
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                echo 'deploying docker image to EC2'
                def dockerCmd = 'docker run -p 8080:8080 -d maxts0gt/jenkins-demo-app:jma-1.0.0' 
                sshagent(['ec2-server-key']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@54.234.144.50 ${dockerCmd}"
                    }
                }
                }
            }
        }
    }
