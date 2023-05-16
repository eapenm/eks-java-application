@Library('eapen-shared-library') _
pipeline{
    agent any
    parameters{
        choice(name: 'action', choices: 'Create\ndelete', description: 'Choose Create/Destroy')
        string(name: 'ImageName',description: "Name of the docker build", defaultValue: 'javapp')
        string(name: 'ImageTag',description: "Tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser',description: "Name of the Application", defaultValue: '9886708510')
    }
    stages{
        stage("GIT Checkout"){
            when {expression {params.action == 'Create'}}
            steps{
               script{
                gitCheckout(
                    branch: 'devops', 
                    url: 'https://github.com/eapenm/eks-java-application.git'
                )
               }
            }
        }
        stage("Unit Testing using Maven"){
            when {expression {params.action == 'Create'}}
            steps{
               script{
                mvnTest()
               }
            }
        }
        stage("Integration Testing using Maven"){
            when {expression {params.action == 'Create'}}
            steps{
               script{
                mvnIntegrationTest()
               }
            }
        }
        stage('Static code analysis: Sonarqube'){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   
                   def sonarQubecredentialsId = 'sonar-token'
                   staticCodeAnalysis(sonarQubecredentialsId)
               }
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   //Qulity gate status check
                   def sonarQubecredentialsId = 'sonar-token'
                   qualityGateStatus(sonarQubecredentialsId)
               }
            }
        }
        stage('Maven Build : maven'){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   
                   mvnBuild()
               }
            }
        }
        stage('Docker Image Build'){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   
                   dockerBuild("${params.ImageName}", "${params.ImageTag}", "${params.DockerHubUser}")
               }
            }
        }
        stage('Docker Image Scan: trivy '){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   
                   dockerImageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
               }
            }
        }
        stage('Docker Image Push : DockerHub '){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                  }
            }
        }   
        stage('Docker Image clean : DockerHub '){
         when { expression {  params.action == 'Create' } }
            steps{
               script{
                   dockerImageCleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                  }
            }
        }  
    }

}

