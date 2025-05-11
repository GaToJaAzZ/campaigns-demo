pipeline {
    agent any
    tools {
        maven 'maven-3.9.6'
    }
    stages {
        stage('Packaging') {
            steps {
                echo 'Packaging..'
                sh 'mvn clean package'
            }
        }
        stage('Copying jar file') {
            steps {
                echo 'Copying jar file..'
                sh 'mv target/*.jar .'
            }
        }
        stage('cleanup') {
            steps {
                sh 'docker system prune -a --volumes --force --filter "label=campaigns-demo-server"'
            }
        }
        stage('build image') {
            steps {
                sh 'docker build -t gatojaazz/campaigns-demo:v1 --label campaigns-demo-server .'
            }
        }
        stage('run container') {
            steps {
                sh '''
                    docker rm -f campaigns-demo-server || true
                    docker run -d --name campaigns-demo-server --label campaigns-demo-server -p 5000:5000 gatojaazz/campaigns-demo:v1
                '''
            }
        }
    }
}
