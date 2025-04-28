pipeline {
    agent any

    stages {
        stage('Clonar Repositorio') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/carlosandresfv2007/mi-app-flask'
            }
        }

        stage('Construir Imagen Docker') {
            steps {
                sh 'docker build -t flask-app .'
            }
        }

        stage('Detener Contenedor Anterior') {
            steps {
                sh 'docker stop flask-container || true'
                sh 'docker rm flask-container || true'
            }
        }

        stage('Desplegar Nueva Versión') {
            steps {
                sh 'docker run -d --name flask-container -p 8090:8090 flask-app'
            }
        }
    }
}
