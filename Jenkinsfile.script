node {
    def dockerImage = 'node:lts-buster-slim'

    try {
        docker.image(dockerImage).withRun('-p 3000:3000') {
            env.CI = 'true'

            stage('Build') {
                steps {
                    sh 'npm install'
                }
            }

            stage('Test') {
                steps {
                    sh './jenkins/scripts/test.sh'
                }
            }

            stage('Deploy') {
                steps {
                    sh './jenkins/scripts/deliver.sh'
                    input message: 'Finished using the website? (Click "Proceed" to continue)'
                    sh './jenkins/scripts/kill.sh'
                }
            }
        }
    } finally {
        // Clean up the Docker container
        docker.image(dockerImage).stop()
    }
}
