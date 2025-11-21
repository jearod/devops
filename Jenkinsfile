pipeline {
  agent any
  options { timestamps() }

  stages {

    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'echo Compilando...'
        sh 'ls -la'
      }
    }

    stage('Test') {
      steps {
        sh '''
          echo Ejecutando tests...
          # Siempre retornar éxito
          exit 1
        '''
      }
    }

    stage('Run script') {
      steps {
        sh 'bash scripts/hola.sh'
      }
    }

    stage('Generate Artifact') {
      steps {
        sh 'echo "Resultado del build #${BUILD_NUMBER}" > build-${BUILD_NUMBER}.txt'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: "build-${env.BUILD_NUMBER}.txt", fingerprint: true
      }
    }
  }

  post {
    success { echo '✅  Todo OK' }
    failure { echo '❌  Algo falló' }
  }
}

