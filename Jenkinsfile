pipeline{
    agent any
    triggers{
        pollSCM ( '* * * * *')
    }
    stages{
        stage( 'VCS'){
            steps{
                git url: 'https://github.com/Gopi0527/nopCommerce.git',
                barnch: 'master'
            }
        }
        stage( 'Build the dotnet'){
            steps{
                sh 'cd nopCommerce '
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