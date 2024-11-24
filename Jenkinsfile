pipeline {
    agent any
    environment {
        // Define variables de entorno necesarias
        NODE_HOME = 'C:\\Program Files\\nodejs' // Ajusta esto según tu entorno
        PATH = "${NODE_HOME};${env.PATH}"
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo 'Clonando el repositorio...'
                git branch: 'main', url: 'https://github.com/perman03/webscraping-insurance'
            }
        }
        stage('Install Dependencies') {
            steps {
                echo 'Instalando dependencias...'
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Ejecutando pruebas con WebdriverIO...'
                bat 'npm run scraping'
            }
        }
        stage('Archive CSV') {
            steps {
                echo 'Archivando el CSV generado...'
                archiveArtifacts artifacts: 'secureCar.csv', fingerprint: true
            }
        }
        stage('Publish Reports and Videos') {
            steps {
                echo 'Publicando artefactos...'
                // Copia videos u otros artefactos al directorio para HTML Publisher
                bat 'mkdir -p target'
                bat 'copy secureCar.csv target/'
                
                publishHTML(target: [
                    allowMissing: false,
                    keepAll: true,
                    reportDir: 'target',
                    reportFiles: 'secureCar.csv',
                    reportName: 'Secure Car Info'
                ])
            }
        }
    }
    post {
        always {
            echo 'Limpieza del workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline completado con éxito.'
        }
        failure {
            echo 'Pipeline fallido. Revisa los logs.'
        }
    }
}
