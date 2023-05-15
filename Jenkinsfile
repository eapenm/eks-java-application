@Library('eapen-shared-library') _
pipeline{
    agent any
        environment {
        JVM_ARGS = '-Xmx1024m --add-opens java.base/java.lang=ALL-UNNAMED'
    }
    stages{
        stage("GIT Checkout"){
            steps{
               script{
                gitCheckout(
                    branch: 'main', 
                    url: 'https://github.com/eapenm/eks-java-application.git'
                )
               }
            }
        }
        stages {
        stage('Build') {
            steps {
                sh 'mvn clean compile -DskipTests=true -Dmaven.compiler.release=11 -Dmaven.compiler.args="--add-opens java.base/java.lang=ALL-UNNAMED"'
            }
        }
    }
        stage("Unit Testing using Maven"){
            steps{
               script{
                mvnTest()
               }
            }
        }
        
    }

}