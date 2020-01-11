pipeline {
    agent any
    stages {
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
                                        sourceFiles: 'web227/index.html' 'web227/pic8.jpg.jpg',
                                        removePrefix: 'web227/',
                                        remoteDirectory: '/var/www/html',
                                        execCommand: "pwd && ls -la /tmp && ls -la /var/www/html"
                                    )
                                ]
                            )
                        ]
                    )
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
