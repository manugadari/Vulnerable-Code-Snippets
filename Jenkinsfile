pipeline {
  agent any
 
  stages {
    stage('check out') {
      steps {
                script {
                    // Fetch the latest changes
                    sh 'git fetch origin'

                    // Get the list of modified files
                    def modifiedFiles = sh(script: "git diff --name-only master feature-1", returnStdout: true).trim()

                    // Print the list of modified files
                    echo "Modified files:\n${modifiedFiles}"
                    
                    // Ensure the output directory exists
                    sh "mkdir -p modified_files"

                    // Copy the modified files to the output directory
                    sh "echo ${modifiedFiles} | xargs -I {} sh -c 'mkdir modified_files/{} 2>nul && cp {} modified_files/{}'"
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
