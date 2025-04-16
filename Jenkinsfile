pipeline {
    agent {
        docker { 
            image 'lochnair/alpine-sdk:latest'
            args '--group-add abuild'
        }
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'doas apk update'
                withEnv(['PACKAGER="Nils Andreas Svee <me@lochnair.net>"', 'REPODEST="$WORKSPACE/repo/"']) {
                    withCredentials([file(credentialsId: 'abuild-privkey', variable: 'PACKAGER_PRIVKEY'), file(credentialsId: 'abuild-pubkey', variable: 'PACKAGER_PUBKEY')]) {
                        sh 'abuild -r'
                    }
                }
            }
        }
        
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'repo/**/*.apk', fingerprint: true, onlyIfSuccessful: true
            }
        }
    }
}
