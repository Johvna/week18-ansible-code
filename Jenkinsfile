pipeline {
    agent any

    stages {
        stage('Zip and Push to JFrog Artifactory') {
            steps {
                script {
                    // Create a zip file containing all files except Jenkinsfile
                    sh 'zip -r ansible-codes.zip * -x Jenkinsfile'
                    
                    // Upload the zip file to JFrog Artifactory
                    sh 'curl -uadmin:APBc9hqNY7jWVLeZK6P3czW7MqA -T ansible-codes.zip "http://100.26.209.109:8081/artifactory/ansible-codes/"'
                }
            }
        }
        
        stage('Download Artifact from Jfrog') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Download the zip file from JFrog Artifactory
                    sh 'curl -uadmin:APBc9hqNY7jWVLeZK6P3czW7MqA -O "http://100.26.209.109:8081/artifactory/ansible-codes/"'
                    // Now you can use the downloaded files on the agent
                }
            }
        }
        
        stage('Run playbook') {
            agent {
                label 'ansible'
            }
            steps {
                script {
                    // Unzip ansible-codes.zip
                    sh 'unzip -o ansible-codes.zip'
                    // Run ansible-playbook from the correct directory
                    dir('ansible-codes') {
                        //sh 'ansible-vault decrypt /home/ec2-user/ansible-dev/workspace/week16-project/ansible-codes/inventory.yml --vault-password-file /home/ec2-user/ansible-dev/workspace/week16-project/ansible-codes/abc123.txt'
                        sh 'ansible-playbook -i /home/ec2-user/ansible-dev/inventory.yml /home/ec2-user/ansible-dev/play1.yml'
                        
                    }
                }
            }
        }
    }
}