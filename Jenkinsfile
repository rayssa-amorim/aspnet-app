
def ReleaseDir = "c:\\inetpub\\wwwroot"

pipeline {
   agent {
      node {
         label 'Windows'
     }
   }
   stages {
      stage('Checkout_app') {
         steps {
            checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/rayssa-amorim/aspnet-app.git']])
         }
      }
      stage('Compilacao_Deploy_app') {
         steps {
            bat """
               D:\\tools\\Nuget\\nuget.exe restore src\\aspnetapp.sln
            """
            bat "\"${tool 'MSBuild'}\" src\\aspnetapp.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=True /p:publishUrl=${ReleaseDir}"
         }
      }
   }
}
