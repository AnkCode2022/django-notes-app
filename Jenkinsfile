pipeline {
    agent any

    stages {
        stage("Clone") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/AnkCode2022/django-notes-app.git/", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t my-notes-app ."
            }
        }
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh '''
                        echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin
                        docker tag my-notes-app ${dockerHubUser}/my-notes-app:latest
                        docker push ${dockerHubUser}/my-notes-app:latest
                    '''
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the Docker container"
                sh "docker-compose up -d"
            }
        }
    }
}
