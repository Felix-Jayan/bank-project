pipeline{
    agent any
    stages{
        stage('checkout the code from github'){
            steps{
                 git url: 'https://github.com/Felix-Jayan/bank-project/'
                 echo 'github url checkout'
            }
        }
        stage('codecompile with felix'){
            steps{
                echo 'starting compiling'
                sh 'mvn compile'
            }
        }
        stage('codetesting with felix'){
            steps{
                sh 'mvn test'
            }
        }
        stage('qa with felix'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('package with felix'){
            steps{
                sh 'mvn package'
            }
        }
        stage('run dockerfile'){
          steps{
               sh 'docker build -t fiya/myproject:1 .'
           }
        }
        stage('Login the docker hub and push the file'){
            steps{
                withCredentials([string(credentialsId: 'dockerhubpassword', variable: 'dockerhubpass')]) {
                    sh 'docker login -u fiya -p ${dockerhubpass}'
                }
            }
        }
        stage('push to docker hub'){
          steps{
               sh 'docker push fiya/myproject:1'
           }
        }
        stage('Diployment stage using ansible'){
          steps{
              ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', sudoUser: null, vaultTmpPath: ''
           }
        }
    }
}
