pipeline {
    agent any
tools{
      maven 'M2_HOME'
          }
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/Augustine78/Healthcare-project.git', branch: "master"
               sh 'mvn clean package'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t augustine325/medicure:1.0 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([string(credentialsId: 'docker', variable: 'dockervar')]){
                    sh 'docker login -u augustine325 -p ${dockervar}'
                    sh 'docker push augustine325/medicure:1.0'
                }
            }
        }

         stage('Ansible Playbook') {
      steps {
        ansiblePlaybook credentialsId: 'sshkey', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
                                       }
            }
       
            
    }
}
