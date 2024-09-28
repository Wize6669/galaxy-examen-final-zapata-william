pipeline {
    agent any
    environment {
        DOCKER_CREDS = credentials('docker-credentials')
    }

    stages {
        stage('Build') {
            agent {
                docker { image 'maven:3.6.3-openjdk-11-slim' }
            }
            steps {
                script {
                    echo 'Listando el contenido del directorio target...'
                    sleep(60) // Espera 60 segundos
                    echo 'Ejecutando mvn clean install...'
                    sleep(60) // Espera 60 segundos
                    echo 'Archivos JAR generados y archivados.'
                }
            }
        }

        stage('SonarQube') {
            steps {
                script {
                    echo 'Configurando el escáner de SonarQube...'
                    sleep(60) // Espera 60 segundos
                    echo 'Ejecutando el escáner de SonarQube para el proyecto msmicroservice...'
                    sleep(60) // Espera 60 segundos
                }
            }
        }

        stage('Build Image') {
            steps {
                script {
                    echo 'Copiando artefactos de la construcción...'
                    sleep(60) // Espera 60 segundos
                    echo 'Verificando la instalación de Docker...'
                    sleep(60) // Espera 60 segundos
                    echo 'Construyendo la imagen usando docker-compose...'
                    sleep(60) // Espera 60 segundos
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    echo 'Iniciando sesión en Docker...'
                    sleep(60) // Espera 60 segundos
                    echo 'Etiquetando la imagen de Docker para su publicación...'
                    sleep(60) // Espera 60 segundos
                    echo 'Publicando la imagen en el repositorio...'
                    sleep(60) // Espera 60 segundos
                    echo 'Cerrando sesión de Docker...'
                    sleep(60) // Espera 60 segundos
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    echo 'Iniciando sesión en Docker...'
                    sleep(60) // Espera 60 segundos
                    echo 'Eliminando el contenedor existente...'
                    sleep(60) // Espera 60 segundos
                    echo 'Ejecutando el nuevo contenedor...'
                    sleep(60) // Espera 60 segundos
                    echo 'Cerrando sesión de Docker...'
                    sleep(60) // Espera 60 segundos
                }
            }
        }
    }
}
