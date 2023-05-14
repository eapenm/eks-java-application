pipeline{
    agent any
    stages{
        stage("GIT Checkout"){
            steps{
               script{
                git branch: 'main', url: 'https://github.com/eapenm/eks-java-application.git'
               }
            }

        }
    }

}