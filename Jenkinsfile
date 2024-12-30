def gv

pipeline {
    agent any
    tools{
        maven 'Maven'
    }
   // comment

    stages {

        stage("init") {

            steps {
                
                echo "Initializing "

                script{
                    gv= load "script.groovy"

                }
            }
        }
        stage("increment version"){
            steps{
                script{
                    echo "Incrementing version" 
                    sh "mvn build-helper:parse-version versions:set \
                    -DnewVersions=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncremtalVersion} versions:commit"
                    def matcher= readFile("pom.xml") =~ "<version>(.+)</version>"
                    def version=matcher[0][1]
                    env.IMAGE_VERSION="$version-$BUILD_NUMBER"
                }
            }
        }
        stage("build jar") {
           steps{
                sh 'mvn clean package'
                
           }
        }
        stage("build image") {
            steps {
                echo "build image"
                withCredentials([
                        usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PWD')
                    ]) {
                    
                    sh "docker build -t starteja007/java-maven-app:$IMAGE_VERSION ."
                    sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
                    sh "docker push starteja007/java-maven-app:$IMAGE_VERSION "

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