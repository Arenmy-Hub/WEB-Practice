pipeline {
    agent any
   
    stages {
        stage('Create web directory') {
            input {
                message 'Enter the data'
                parameters {
                    string(name: 'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment')
                    string(name: 'ENVIRONMENT', defaultValue: 'Development', description: 'Environment to deploy')
                }
            }
            steps {
                echo "The responsible of this project is ${AUTHOR} and will be deployed in ${ENVIRONMENT}"
                // Elimina el directorio si existe
                sh 'rm -rf /home/jenkins/web'
                // Crea la ruta completa de carpetas padre si no existen
                sh 'mkdir -p /home/jenkins/web'
            }
        }
        stage('Drop the Apache HTTPD Docker container') {
            steps {
                echo 'Droping the container...'
                sh 'docker rm -f apache1 || true' // '|| true' evita que falle si el contenedor no existe previamente
            }
        }
        stage('Create the Apache httpd container') {
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name apache1 -p 9000:80 -v /home/jenkins/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                sh 'cp -r web/* /home/jenkins/web'
            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'wget http://localhost:9000'
            }
        }        
    }
}
