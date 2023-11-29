pipeline{
    agent any
    triggers{
        cron ( '*/30 * * * *')
    }
    stages{
        stage( 'VCS'){
            steps{
                git url: 'https://github.com/Gopi0527/nopCommerce.git',
                branch: 'master'
            }
        }
        stage( 'Build the dotnet'){
            steps{
                sh 'dotnet build -c Release src/NopCommerce.sln'
                sh 'dotnet publish -c Release src/Presentation/Nop.Web/Nop.Web.csproj  -o "./published"'
            }
            post{
                success{
                    sh 'mkdir ./published/bin ./published/logs'
                    sh 'tar -czvf nop.web.tar.gz ./published/'
                    archiveArtifacts  artifacts: '**/*.tar.gz'
                } 
                failure{
                    mail subject: 'build stage failure',
                         to: 'krishna@qt.com',
                         from: 'krishna@qt.com',
                         body:  "Refer to $BUILD_URL for more details"
                }
                
            }
        }


    }
}