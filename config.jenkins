pipeline {
    agent any
    environment {
        // Define variables de entorno necesarias
        NODE_HOME = 'Users/frank/AppData/Roaming/nvm' // Ajusta esto según tu entorno
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
                sh 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Ejecutando pruebas con WebdriverIO...'
                sh 'npm run scraping'
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
                sh 'mkdir -p target'
                sh 'cp secureCar.csv target/'
                
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
