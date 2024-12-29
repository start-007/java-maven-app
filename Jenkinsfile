def gv

pipeline {
    agent any
    tools{
        maven 'Maven'
    }
   

    stages {
        stage("init") {
            steps {
                
                echo "Initializing "

                script{
                    gv= load "script.groovy"

                }
            }
        }
        stage("build jar") {
           steps{
                sh 'mvn package'
           }
        }
        stage("build image") {
            steps {
                echo "build image"
                withCredentials([
                        usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PWD')
                    ]) {
                    
                    sh "docker build -t starteja007/java-maven-app:1.0 ."
                    sh "sudo usermod -a -G docker jenkins"
                    sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
                    sh "docker push starteja007/java-maven-app:1.0 "

                }
            }
        }
        stage("deploy") {
           
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