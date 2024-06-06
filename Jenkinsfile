pipeline {
  agent any

  stages {
    stage('check out') {
      steps {
        git 'https://github.com/manugadari/Vulnerable-Code-Snippets'
        def modifiedFiles = sh(script: "git diff --name-only master feature", returnStdout: true).trim(){
        sh'git diff --name-only feature-1 master'
        }
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
                sh 'snyk code test git diff --name-only master feature-1'
                }
            }
    
        stage('SAST SCAN for all files') {
            steps {
                sh 'snyk code test '
                }
          }
     }
}
