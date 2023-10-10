pipeline {
  agent any

  options {
    ansiColor('xterm')
  }

  parameters {
    string(name: 'ENV', defaultValue: 'prod', description: 'Environment')
    string(name: 'APPNAME', defaultValue: '', description: 'App Name')
    string(name: 'APP_VERSION', defaultValue: '', description: 'APP Version')
  }

  stages {

    stage('Get KubeConfig') {
      steps {
        sh 'aws eks update-kubeconfig --name ${ENV}-roboshop'
      }
    }

    stage('Get App Code') {
      steps {
        dir('APP') {
          git branch: 'main', url: 'https://github.com/raghudevopsb74/${APPNAME}'
        }
        dir('CHART') {
          git branch: 'main', url: 'https://github.com/raghudevopsb74/roboshop-helm'
        }
      }
    }

    stage('Helm Deploy') {
      steps {
        sh 'helm upgrade -i ${APPNAME} ./CHART -f APP/helm/${ENV}.yaml --set APP_VERSION=${APP_VERSION}'
      }
    }

  }
}