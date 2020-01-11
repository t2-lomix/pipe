pipeline {
    agent any
    stages {
        stage('Build tar archive') {
            steps {
                echo 'Running build tar'
                execCommand: 'cd web227/ && tar -cvzf web227.tar.gz index.html pic8.jpg.jpg && cd .. && cd web327/ && tar -cvzf web327.tar.gz index.html pic8.jpg.jpg'
            }
        }
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: "team2-ssh_node1", keyFileVariable: 'secret.key')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'web227',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/web227.tar.gz',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/var/www/html/',
                                        execCommand: 'tar -xvzf web227.tar.gz && sudo service httpd restart'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
        stage('DeployToOneMoreServer') {
            when {
                branch 'master'
            }
            steps {
                input 'Does the two servers look OK?'
                milestone(1)
                withCredentials([sshUserPrivateKey(credentialsId: "team2-ssh_node1", keyFileVariable: 'secret.key')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'web227',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/web327.tar.gz',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/var/www/html/',
                                        execCommand: 'tar -xvzf web327.tar.gz && sudo service httpd restart'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
