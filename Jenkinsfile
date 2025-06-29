pipeline {
  agent any

  options {
    buildDiscarder(logRotator(numToKeepStr: '10'))
    disableConcurrentBuilds()
  }

  triggers {
    pollSCM('H/5 * * * *')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Restore') {
      when {
        branch 'main'
      }
      steps {
        bat 'dotnet restore'
      }
    }

    stage('Build') {
      when {
        branch 'main'
      }
      steps {
        bat 'dotnet build --configuration Release --no-restore'
      }
    }

    stage('Test') {
      when {
        branch 'main'
      }
      steps {
        bat 'dotnet test --no-build --verbosity normal'
      }
    }
  }

  post {
    always {
      echo "Pipeline completed on branch: ${env.BRANCH_NAME}"
    }
  }
}