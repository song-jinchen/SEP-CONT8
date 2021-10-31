pipeline {
    agent any

    tools {
        nodejs "nodejs"
    }

    stages {
        stage('Pull') {
            steps {
                git branch: 'main', credentialsId: 'Crazyorchid', url: 'https://github.com/Crazyorchid/SEP-CONT8.git'
            }
        }

        stage('Install') {
            steps {
                sh '''
                npm install --registry=https://registry.npm.taobao.org
                npm install core-js@2.6.5 --save --registry=https://registry.npm.taobao.org
                '''
            }
        }

        stage('Build') {
            steps {
                script {
                    if (params.env == 'all' || params.env == 'online-test') {
                        sh '''npm run onlinetest'''
                    }

                    if (params.env == 'all' || params.env == 'online-test') {
                        sh '''npm run package'''
                    }
                }
            }
        }

        stage('Upload') {
            steps {
                script {
                    if (params.env == 'all' || params.env == 'online-test') {
                        sshPublisher(publishers: [sshPublisherDesc(configName: 'ssh-config-name', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/app/a-application-test', remoteDirectorySDF: false, removePrefix: 'dist/prod', sourceFiles: 'dist/prod/**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                    }
                    if (params.env == 'all' || params.env == 'product') {
                        sshPublisher(publishers: [sshPublisherDesc(configName: 'ssh-config-name', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/app/a-application', remoteDirectorySDF: false, removePrefix: 'dist/prod', sourceFiles: 'dist/prod/**')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                    }
                }
            }
        }
    }
}