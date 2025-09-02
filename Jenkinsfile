pipeline {
    agent any // This defines where the pipeline will run

    options {
        timeout(time: 10, unit: 'MINUTES') // Prevent jobs from hanging
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm // This checks out the code from GitHub
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbook.yml',
                    inventory: 'hosts',
                    credentialsId: '', // Leave empty since we're using SSH keys directly
                    disableHostKeyChecking: true, // For lab use only! Not for production.
                    colorized: true
                )
            }
        }
    }

    post {
        always {
            echo 'Pipeline for updating lab servers has completed.'
            // You can add email notifications here later
            // emailext body: "Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} result: ${currentBuild.currentResult}", subject: "Jenkins Build Result", to: "you@email.com"
        }
        success {
            echo 'The playbook executed successfully on all hosts!'
        }
        failure {
            echo 'The playbook failed on one or more hosts. Check the logs.'
        }
    }
}
