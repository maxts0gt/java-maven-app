#!/usr/bin/env groovy

pipeline {
    agent any
    stages {
        stage("test") {
            steps {
                 echo 'testing the application'
                echo 'building the application'
            }
        }
        stage("build") {
            steps {
                echo 'building the application'
            }
        }
        stage("deploy") {
            steps {
                script {
                def dockerCmd = 'docker run -p 8080:8080 -d maxts0gt/jenkins-demo-app:jma-1.0.0' 
                sshagent(['ec2-server-key']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@54.234.144.50 ${dockerCmd}"
                    }
                }
                }
            }
        }
    }
