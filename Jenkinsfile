@Library('eapen-shared-library') _
pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
    }
    stages{
        stage("GIT Checkout"){
            when {expression {param.action == 'create'}}
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
            when {expression {param.action == 'create'}}
            steps {
                script{
                mvnCleanCompile()
                }
            }
        }
        stage("Unit Testing using Maven"){
            when {expression {param.action == 'create'}}
            steps{
               script{
                mvnTest()
               }
            }
        }
        stage("Integration Testing using Maven"){
            when {expression {param.action == 'create'}}
            steps{
               script{
                mvnIntegrationTest()
               }
            }
        }
        stage('Static code analysis: Sonarqube'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   def SonarQubecredentialsId = 'sonar-token'
                   staticCodeAnalysis(SonarQubecredentialsId)
               }
            }
        }
        // stage('Quality Gate Status Check : Sonarqube'){
        //  when { expression {  params.action == 'create' } }
        //     steps{
        //        script{
                   
        //            def SonarQubecredentialsId = 'sonar-token'
        //            qualityGateStatus(SonarQubecredentialsId)
        //        }
        //     }
        // }
        stage('Maven Build : maven'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
        
    }

}

