pipeline {
    agent any

    stages {
        stage("Code Checkout") {
            steps {
                git url: "https://github.com/AYUSH-HEDAOO/two-tier-flask-app.git", branch: "master"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubcreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {
                    sh '''
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker tag two-tier-flask-app "$dockerHubUser/two-tier-flask-app:latest"
                        docker push "$dockerHubUser/two-tier-flask-app:latest"
                    '''
                }
            }
        }

        stage("Test") {
            steps {
                echo "Code test completed ✅"
            }
        }

        stage("Deploy") {
            steps {
                sh 'docker compose up -d --build'
            }
        }
    }
}
