pipeline {
  agent any
 
  stages {
    stage('Build') {
      steps {
        git 'https://github.com/manugadari/Vulnerable-Code-Snippets'
        git branch: 'feature-1', url: 'https://github.com/manugadari/Vulnerable-Code-Snippets'{
        sh ''' for /F "delims=" %f in ('git diff --name-only master feature-1') do (
    mkdir "modified_files\%~pf" 2>nul
    copy "%f" "modified_files\%f") '''
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
