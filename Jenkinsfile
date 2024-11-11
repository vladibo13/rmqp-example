#!/usr/bin/env groovy
library identifier: 'jenkins-shared-lib@main', retriever: modernSCM(
    [$class: 'GitSCMSource',
     remote: 'https://github.com/vladibo13/jenkins-shared-libary.git',
     credentialsId: 'github-secret'
    ]
)

pipeline {
    agent any

     environment { 
        PRODUCER_IMAGE_NAME = 'vladibo/rmq-producer:1.0' 
        PRODUCER_DOCKER_FILE_PATH = 'producer/Dockerfile'  
        PRODUCER_CONTEXT_DIR = 'producer/'

        CONSUMER_IMAGE_NAME = 'vladibo/rmq-consumer:1.0' 
        CONSUMER_DOCKER_FILE_PATH = 'consumer/Dockerfile'  
        CONSUMER_CONTEXT_DIR = 'consumer/'        
    }


    stages{
        stage("build docker images") {
            steps {
                script {
                  echo "building producer image..."
                  buildImageWithFilePath(env.PRODUCER_DOCKER_FILE_PATH, env.PRODUCER_CONTEXT_DIR, env.PRODUCER_IMAGE_NAME)

                  echo "building consumer image..."
                  buildImageWithFilePath(env.CONSUMER_DOCKER_FILE_PATH, env.CONSUMER_CONTEXT_DIR, env.CONSUMER_IMAGE_NAME)
                }
            }
        }

        stage("push docker images to hub registry") {
            steps {
                script {
                   echo "docker hub login..."
                   dockerLogin()

                   echo "pushing producer image to docker hub..."
                   dockerPush(env.PRODUCER_IMAGE_NAME)

                   echo "pushing consumer image to docker hub.."
                   dockerPush(env.CONSUMER_IMAGE_NAME)
                }
            }
        }

        stage("test") {
            steps {
                script {
                   echo "testing the app"
                }
            }
        }
        stage("deploy") {
            steps {
                script {
                   echo "deploying the app..."
                }
            }
        }
    }
}