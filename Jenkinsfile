pipeline {
    agent any
    environment{
        VERSION = "1.9.0"
        SERVER_CREDENTIALS= credentials("github-credentials")
    }
    parameters{
        choice(name:"choice", choices:["1.2.0","2.3.4","2.32"],description:"")
        booleanParam(name:"expected", defaultValue:true,description:"")
    }
    stages {
        stage("init") {
            steps {
                echo "Initializing "
                echo "Current version ${VERSION}"
            }
        }
        stage("build jar") {
            when{
                expression{
                    params.expected
                }
            }
            steps {
               echo "building jar"
               echo "Global scoped credentials ${SERVER_CREDENTIALS_USR} ${SERVER_CREDENTIALS_PSW}"
                echo "Got global credentials"
            }
        }
        stage("build image") {
            steps {
                echo "build image"
                withCredentials([
                        usernamePassword(credentialsId: 'server-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')
                    ]) {
                        echo "user: ${USER}, password: ${PWD}"
                        echo "Got credentials"
                }
            }
        }
        stage("deploy") {
           
            steps {
                echo "deploying"
                echo "Deploying version ${params.choice}"
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