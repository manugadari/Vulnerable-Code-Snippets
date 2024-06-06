pipeline {
  agent any

  stages {
    stage('check out') {
      steps {
        git 'https://github.com/manugadari/Vulnerable-Code-Snippets'
        git branch: 'feature-1', url: 'https://github.com/manugadari/Vulnerable-Code-Snippets'
        sh'git checkout feature-1'
        sh'git diff --name-only feature-1 master >> mkdir -p changes '
      }
    } 
    stage('Authorize Snyk CLI') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'snyk auth ${SNYK_TOKEN}'
                }
             }
          }

        stage('SAST SCAN for modified files') {
            steps {
                sh 'snyk code test changes '
                }
            }
    
        stage('SAST SCAN for all files') {
            steps {
                sh 'snyk code test '
                }
          }
     }
}
