pipeline {
    agent {
        docker { 
            image 'lochnair/alpine-sdk:edge'
            args '--group-add abuild'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'doas apk update'
                withEnv(['PACKAGER=Nils Andreas Svee <me@lochnair.net>', "REPODEST=$WORKSPACE/repo/"]) {
                    withCredentials([file(credentialsId: 'abuild-privkey', variable: 'PACKAGER_PRIVKEY'), file(credentialsId: 'abuild-pubkey', variable: 'PACKAGER_PUBKEY')]) {
                        sh 'doas cp -v $PACKAGER_PUBKEY /etc/apk/keys/'
                        sh 'doas chmod 644 /etc/apk/keys/*'
                        sh 'abuild -r'
                        sh 'cp -v $PACKAGER_PUBKEY repo/'
                    }
                }
            }
        }
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'repo/**/*.apk', fingerprint: true, onlyIfSuccessful: true
                archiveArtifacts artifacts: 'repo/*.pub', fingerprint: true, onlyIfSuccessful: true
            }
        }
    }

    post {
        always {
            cleanWs cleanWhenFailure: false
        }
    }

}
