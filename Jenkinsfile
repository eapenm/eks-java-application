@Library('eapen-shared-library') _
pipeline{
    agent any
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
        stage('Build') {
            steps {
                script{
                sh 'mvn clean compile'
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