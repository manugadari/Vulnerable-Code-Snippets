pipeline {
  agent any

  stages {
    stage('check out') {
      steps {
        git 'https://github.com/manugadari/Vulnerable-Code-Snippets'
        sh'git diff --name-only feature-1 master'
      }
    } 
    stage('Get Modified Files') {
            steps {
                script {
                    // Fetch the latest changes
                    sh 'git fetch origin'

                    // Get the list of modified files
                    def modifiedFiles = sh(script: "git diff --name-only master feature-1", returnStdout: true).trim()

                    // Copy the modified files to the output directory
                    modifiedFiles.split('\n').each { file ->
                        def dirPath = file.contains('/') ? file.substring(0, file.lastIndexOf('/')) : ''
                        sh """
                            mkdir -p modified_files/${dirPath}
                            cp --parents ${file} modified_files/
                        """
                    }
                }
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
