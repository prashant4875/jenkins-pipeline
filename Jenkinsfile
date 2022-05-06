def awsCredentials = [[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-access']]

pipeline {
  agent any

  environment {
    HELLO_WORLD = "Hello world!"
  }

  options {
    disableConcurrentBuilds()
    parallelsAlwaysFailFast()
    timestamps()
    withCredentials(awsCredentials)
  }

  stages {
    stage('Git Checkout'){
      steps{
        git credentialsId: '62786928-4f0d-40bb-9a95-1b5deae0a654', url: 'https://github.com/prashant4875/jenkins-pipeline.git'
        sh 'git checkout master'
        sh 'git pull https://github.com/prashant4875/jenkins-pipeline.git master'
      }
    }

    stage('Initial Steps'){
      steps{
        sh 'npm install -g aws-cdk typescript'
      }

    }
    stage('Build & Say Hello World') {
      parallel {
        stage('Build') {
          steps {
            sh 'npm install'
            sh 'npm run build'
          }
        }

        stage('Say Hello World') {
          steps {
            echo "${env.HELLO_WORLD_ENV_VAR}"
          }
        }
      }
    }

    stage('Deploy') {
      steps {
        sh 'cdk ls'
        sh 'cdk deploy --require-approval=never'
      }
    }

    
  }

  post {
    always {
      cleanWs()
    }
  }
}
