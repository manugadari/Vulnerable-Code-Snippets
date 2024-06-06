pipeline {
  agent any

  stages {
    stage('check out') {
      steps {
        git 'https://github.com/manugadari/Vulnerable-Code-Snippets'
        sh'''git diff --name-only feature-1 master'
        '''
           for /F "delims=" %f in ('git diff --name-only main feature') do (
                mkdir "modified_files\%~pf" 2>nul
                copy "%f" "modified_files\%f")   '''

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
