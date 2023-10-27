pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build DockerImage') {
             when {
                branch 'master'
            }
             steps {
                scripts {
                    app = docker.build("mohaqorob/train-schedule")
                    app.inside {
                        sh 'echo $(curl http://127.0.0.1:8080)'
                    }
                }
            }
        }
         stage('Push Docker Image') {
             when {
                branch 'master'
            }
             steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
