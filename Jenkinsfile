pipeline {
    agent any
    environment{
        VERSION = "1.9.0"
        SERVER_CREDENTIALS= credentials("server-credentials")
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
               sh "Global scoped credentials ${SERVER_CREDENTIALS}"
            }
        }
        stage("build image") {
            steps {
                echo "build image"
                withCredentials([
                    usernamePassword(credentails:'server-credentials',usernameVariable:USER,passwordVariable:PWD)
                ]){
                    sh " user- ${USER} password- ${PWD}"
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