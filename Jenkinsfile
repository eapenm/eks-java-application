@Library('eapen-shared-library') _
pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'create\ndelete', description: 'Choose create/Destroy')
        string(name: 'ImageName',description: "Name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag',description: "Tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser',description: "Name of the Application", defaultValue: '9886708510')
    }
    stages{
        stage("GIT Checkout"){
            when {expression {params.action == 'create'}}
            steps{
               script{
                gitCheckout(
                    branch: 'main', 
                    url: 'https://github.com/eapenm/eks-java-application.git'
                )
               }
            }
        }
        // stage("Unit Testing using Maven"){
        //     when {expression {params.action == 'create'}}
        //     steps{
        //        script{
        //         mvnTest()
        //        }
        //     }
        // }
        // stage("Integration Testing using Maven"){
        //     when {expression {params.action == 'create'}}
        //     steps{
        //        script{
        //         mvnIntegrationTest()
        //        }
        //     }
        // }
        // stage('Static code analysis: Sonarqube'){
        //  when { expression {  params.action == 'create' } }
        //     steps{
        //        script{
                   
        //            def sonarQubecredentialsId = 'sonar-token'
        //            staticCodeAnalysis(sonarQubecredentialsId)
        //        }
        //     }
        // }
        // stage('Quality Gate Status Check : Sonarqube'){
        //  when { expression {  params.action == 'create' } }
        //     steps{
        //        script{
        //            //Qulity gate status check
        //            def sonarQubecredentialsId = 'sonar-token'
        //            qualityGateStatus(sonarQubecredentialsId)
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
        stage('Docker Image Build'){
         when { expression {  params.action == 'create' } }
            steps{
               script{
                   
                   dockerBuild("${params.ImageName}", "${params.ImageTag}", "${params.DockerHubUser}")
               }
            }
        }
        
    }

}

