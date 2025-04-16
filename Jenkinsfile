pipeline {
    agent {
        docker { image 'lochnair/alpine-sdk:latest' }
    }
    
    stages {
        stage('Build') {
            steps {
                withEnv(['PACKAGER="Nils Andreas Svee <me@lochnair.net>"', 'REPODEST="$WORKSPACE/repo/"']) {
                    withCredentials([file(credentialsId: 'abuild-privkey', variable: 'PACKAGER_PRIVKEY')]) {
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
