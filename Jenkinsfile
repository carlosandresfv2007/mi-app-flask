pipeline {
    agent any

    environment {
        // Configuración de puertos (ajusta si es necesario)
        DOCKER_IMAGE = 'flask-app'
        CONTAINER_NAME = 'flask-container'
        FLASK_PORT = '8090'
    }

    stages {
        stage('Clonar Repositorio') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/carlosandresfv2007/mi-app-flask.git']]
                ])
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} .'
            }
        }

        stage('Detener y Eliminar Contenedor Anterior') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Desplegar Nueva Versión') {
            steps {
                sh 'docker run -d --name ${CONTAINER_NAME} -p ${FLASK_PORT}:${FLASK_PORT} ${DOCKER_IMAGE}'
            }
        }
    }

    post {
        success {
            echo "🚀 ¡Despliegue exitoso! Accede a: http://${env.JENKINS_URL.split(':')[0].replace('http://','')}:${FLASK_PORT}"
        }
        failure {
            echo '❌ Error en el despliegue. Revisa los logs:'
            sh 'docker logs ${CONTAINER_NAME} --tail 50 || true'
        }
    }
}
