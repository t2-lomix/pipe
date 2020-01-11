pipeline {
    agent any
    stages {
        stage('Build tar archive') {
            steps {
                echo 'Running build tar'
                sh 'cd web227/ && tar -cvzf web227.tar.gz index.html pic8.jpg.jpg && cd .. && cd web327/ && tar -cvzf web327.tar.gz index.html pic8.jpg.jpg'
            }
        }
        stage('DeployToWeb227') {
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
                                        sourceFiles: 'web227/web227.tar.gz',
                                        removePrefix: 'web227/',
                                        remoteDirectory: '/tmp/',
                                        execCommand: "pwd && ls -la /tmp && ls -la /var/www/html"
                                    )
                                ]
                            )
                        ]
                    )
                fileOperations([fileUnTarOperation(filePath: '/tmp/web227.tar.gz', isGZIP: true, targetLocation: '/tmp/')])   
                }
            }
        }
        stage('DeployToWeb327') {
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
                                configName: 'web327',
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'web327/web327.tar.gz',
                                        removePrefix: 'web327/',
                                        remoteDirectory: '/var/www/html/',
                                        execCommand: 'tar -xvzf web327.tar.gz -C /var/www/html && sudo service httpd restart'
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
