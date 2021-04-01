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
                script {
                    withCredentials([usernamePassword(credentialsId: 'harbor_hub_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')])
                    sh "docker login -u $USERNAME --password-stdin 'USERPASS' https://18.197.69.199/"
                    echo '${env.BUILD_NUMBER}'
                    echo 'latest"'
                        
                }
            }
        }
    }
}
