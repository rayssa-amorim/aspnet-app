
def ReleaseDir = "c:\\inetpub\\wwwroot"

pipeline {
   agent {
      node {
         label 'Windows'
      }
   }
   options {
	   timeout(time: 1, unit: 'DAYS')
        //Quantos builds ele deve mostrar no histórico
		buildDiscarder(logRotator(numToKeepStr: '3', artifactNumToKeepStr: '3')) 
       //Pular checkout padrão
      skipDefaultCheckout()
      }
   stages {
      stage('Checkout Github') {
         steps {
            checkout scmGit(
               branches: [[name: 'master']], 
               browser: github('https://github.com/rayssa-amorim/aspnet-app.git'), 
               extensions: 
                    [cloneOption(honorRefspec: true, noTags: true, reference: '', shallow: false), lfs(), localBranch('master')], 
               userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/rayssa-amorim/aspnet-app.git']])
         }
      }
      stage('Compilacao_Deploy_App') {
         steps {
            bat """
               D:\\tools\\Nuget\\nuget.exe restore src\\aspnetapp.sln
            """
            bat "\"${tool 'MSBuild'}\" src\\aspnetapp.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=True /p:publishUrl=c:\\inetpub\\wwwroot"
            }
         }
      }
   } 
