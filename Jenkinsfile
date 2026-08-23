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
                // Trabajar dentro del workspace de Jenkins evita problemas de permisos
                sh 'rm -rf ./web_dir'
                sh 'mkdir -p ./web_dir'
            }
        }
        stage('Drop the Apache HTTPD Docker container') {
            steps {
                echo 'Droping the container...'
                sh 'docker rm -f apache1 || true'
            }
        }
        stage('Create the Apache httpd container') {
            steps {
                echo 'Creating the container...'
                // Se mapea la ruta absoluta del workspace actual (${WORKSPACE}/web_dir)
                sh 'docker run -dit --name apache1 -p 9000:80 -v ${WORKSPACE}/web_dir:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                sh 'cp -r web/* ./web_dir/'
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
