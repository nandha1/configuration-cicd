pipeline {
    agent any
    tools {
        maven "maven"
    }

    stages {
        stage('SCM checkout') {
            steps {
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nandha1/configuration-cicd.git']])
            }
        }

        stage ('build'){
            steps{
                script{
                    bat 'mvn clean install'
                }
            }
        }

        stage('deploy to war'){
            steps{
               deploy adapters: [tomcat9(credentialsId: 'tomcat-pwd', path: '', url: 'http://localhost:9090/')], contextPath: '/', war: '**/*.war'
            }
        }
    }

        post{
        always{
            emailext attachLog: true,
            body: ''' <html>
    <body>
        <p>Build Status: ${BUILD_STATUS}</p>
        <p>Build Number: ${BUILD_NUMBER}</p>
        <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
    </body>
</html>''', mimeType: 'text/html', replyTo: 'nandha572@gmail.com', subject: 'Pipeline Status : ${BUILD_NUMBER}', to: 'nandha572@gmail.com'

        }
    }
}
