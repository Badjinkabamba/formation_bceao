pipeline {
    agent any

    stages {
        stage('Connect to github') {
            steps {
               git(
                   url: "https://github.com/Badjinkabamba/formation_bceao.git",
                   branch: "main"
               )
            }
        }

        stage('Build Artefact') {
            steps {
               sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Lancer les tests') {
            steps {
               sh 'mvn test'
            }
        }

        stage('Build and Push') {
            steps {
                withDockerRegistry([credentialsId: "my-docker-hub", url: ""]) {
                    sh 'docker build -t badjinkabamba/demo1-v1:$BUILD_NUMBER .'
                    sh 'docker push badjinkabamba/demo1-v1:$BUILD_NUMBER'
                }

            }
        }

        stage('Deploy to Dev') {
            when { expression { false } }
            steps {
               sh 'docker stop demo1-v1 || true'
               sh 'docker rm demo1-v1 || true'
               sh 'docker run -p 8180:8180 -d --name demo1-v1 demo1-v1'
            }
        }
    }
}
