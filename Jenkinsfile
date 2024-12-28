pipeline {
    agent any
    environment{
        VERSION = "1.9.0"
        SERVER_CREDENTIALS= credentials("github-credentials")
    }
    stages {
        stage("init") {
            steps {
                echo "Initializing "
                echo "Current version ${VERSION}"
            }
        }
        stage("build jar") {
            steps {
               echo "building jar"
               sh "Global scoped credentials ${SERVER_CREDENTIALS_USR} ${SERVER_CREDENTIALS_PSW}"
                echo "Got global credentials"
            }
        }
        stage("build image") {
            steps {
                echo "build image"
                withCredentials([
                    usernamePassword(credentials:'server-credentials',usernameVariable:'USER',passwordVariable:'PWD')
                ]){
                   echo " user- ${USER} password- ${PWD}"
                   echo "got credentials"
                }
            }
        }
        stage("deploy") {
            when{
                expression{
                    CODE_CHANGES==true
                }
            }
            steps {
                echo "deploying"
            }
        }
    }   
    post{
        success{
            echo "Well written"
        }
        failure{
            echo "Failed"
        }
    }
}