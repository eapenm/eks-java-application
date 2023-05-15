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
        stage('Build Maven Clean and Compile') {
            steps {
                script{
                mvnCleanCompile()
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
        stage("Integration Testing using Maven"){
            steps{
               script{
                mvnIntegrationTest()
               }
            }
        }
        
    }

}

