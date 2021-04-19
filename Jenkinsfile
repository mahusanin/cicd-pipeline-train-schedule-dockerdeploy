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
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("cloudyrion/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'harbor_hub_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                script {
                    docker.withRegistry('https://173151801028.dkr.ecr.eu-central-1.amazonaws.com', 'AWS_Login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }   
                }
            }
        }
    }
}
