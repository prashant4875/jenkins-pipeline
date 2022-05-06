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
        git 'https://gitlab.com/bhadoria1998/declarative-jenkins.git'
        sh 'git checkout master'
        sh 'git pull https://gitlab.com/bhadoria1998/declarative-jenkins.git master'
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
