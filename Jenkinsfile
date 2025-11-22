pipeline {
    agent any

    parameters {
        string(
            name: 'APP_VERSION',
            defaultValue: 'latest',
            description: 'Versión/tag de la imagen Docker'
        )
        booleanParam(
            name: 'DO_BUILD',
            defaultValue: true,
            description: '¿Construir la imagen Docker?'
        )
        booleanParam(
            name: 'DO_PUSH',
            defaultValue: true,
            description: '¿Hacer push a DockerHub?'
        )
    }

    environment {
        DOCKERHUB_USER = 'tuUsuarioDockerHub'
        IMAGE_NAME     = 'ceste-demo-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Tests / Scripts') {
            steps {
                sh '''
                    echo "Ejecutando tests de ejemplo..."
                    sleep 2
                    echo "Tests OK"
                '''
            }
        }

        stage('Build Docker') {
            when {
                expression { params.DO_BUILD }
            }
            steps {
                sh """
                    echo "Construyendo imagen Docker..."
                    docker build -t ${env.DOCKERHUB_USER}/${env.IMAGE_NAME}:${params.APP_VERSION} .
                """
            }
        }

        stage('Push DockerHub') {
            when {
                allOf {
                    expression { params.DO_PUSH }
                    expression { params.DO_BUILD }
                    branch 'main'
                }
            }
            steps {
                sh """
                    echo "Subiendo imagen a DockerHub..."
                    docker push ${env.DOCKERHUB_USER}/${env.IMAGE_NAME}:${params.APP_VERSION}
                """
            }
        }
    }
}
