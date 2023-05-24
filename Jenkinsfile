//def ReleaseDir = "c:/inetpub/wwwroot"
pipeline {
   agent any
   stages {
      stage('Compilacao') {
         steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_ssh', url: 'https://github.com/rayssa-amorim/aspnet-app.git']]])
         }
      }
      stage('Deploy') {
         steps {
            bat """
               D:\\tools\\Nuget\\nuget.exe restore src\\aspnetapp.sln
            """
            bat "\"${tool 'MSBuild'}\" src\\aspnetapp.sln /p:DeployOnBuild=true /p:DeployDefaultTarget=WebPublish /p:WebPublishMethod=FileSystem /p:SkipInvalidConfigurations=true /t:build /p:Configuration=Release /p:Platform=\"Any CPU\" /p:DeleteExistingFiles=True /p:publishUrl=c:\\inetpub\\wwwroot"
         }
      }
   }
}
