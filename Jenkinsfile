pipeline {
    agent any

    stages {
        stage("Code clone from git") {
            steps {
                echo "this is the code session"
                git url: "https://github.com/kundan4062/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("push to dockerhub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"docker-hub-cred",
                    usernameVariable:"dockerHubUser",
                    passwordVariable:"dockerHubPass")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                sh "docker push ${env.dockerHubUser}/two-tier-flask-app"
                    }
            }
        }
        stage("Test"){
            steps{
                echo "This is Test step"
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
