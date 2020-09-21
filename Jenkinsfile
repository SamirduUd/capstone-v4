@Library('github.com/releaseworks/jenkinslib') _

pipeline {
  environment {
    registry = '8if8troin6i4rv2p/capstone-v3'
    dockerCredential = 'dockerhub-user'
    dockerImage = ''
    //awsCredentials = 'aws-key'
  }

  agent any
  stages {
    stage('Cloning Capstone Project from Github') {
      steps {
        git 'https://github.com/SamirduUd/capstone-v4.git'
      }
    }
/*
    stage('Lint HTML') {
      steps {
        sh 'tidy -q -e index.html'
      }
    }

    stage('Building Docker Image') {
      steps {
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry( '', dockerCredential ) {
            dockerImage.push()
          }
        }

      }
    }

    stage('Create AWS network') {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        AWS("--region=us-east-2 cloudformation create-stack --stack-name CreateNetwork --template-body file://network.yml --parameters file://network-params.json --capabilities CAPABILITY_NAMED_IAM")
        }
      }
    }
*/
    stage('Create EC2 Machines') {
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        AWS("--region=us-east-2 cloudformation create-stack --stack-name CreateHosts --template-body file://servers.yml --parameters file://servers-params.json --capabilities CAPABILITY_NAMED_IAM")
        }
      }
    }

    stage('Cleaning up') {
/*      steps {
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
      steps {
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        AWS("--region=us-east-2 cloudformation delete-stack --stack-name CreateNetwork")
      }
      }
*/
      steps {
          withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws-key', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
          
          //AWS("--region=us-east-2 cloudformation delete-stack --stack-name CreateNetwork")

          AWS("--region=us-east-2 cloudformation delete-stack --stack-name CreateHosts")
          }
        }
      }
    }
  }