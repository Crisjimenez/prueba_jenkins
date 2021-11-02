pipeline {
    agent any
    stages {
        stage('build sin test') { 
            steps {
                nodejs(nodeJSInstallationName: 'nodejs15') { 
                    sh 'npm install'
                    sh 'npm rebuild'
                    sh 'npm run build --skip-test --if-present'
// stash name: "ws", includes: "**"
                }
            }
        }


        stage('unitTest') { 
            steps {
            //unstash "ws"
            nodejs(nodeJSInstallationName: 'nodejs15') {
                sh 'npm run test:coverage && cp coverage/lcov.info lcov.info || echo "Code coverage failed"'
                archiveArtifacts(artifacts: 'coverage/**', onlyIfSuccessful: true)
                }
            }
        }


        stage('deploy') {
            steps {
                nodejs(nodeJSInstallationName: 'nodejs15') {
                    withAWS(credentials: 'aws-credentials') { 
                        sh 'serverless deploy'
                    }
                }       
            }
        }
    }

}
