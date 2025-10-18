pipeline {
  agent any
 
  stages {
    stage('CheckOut') {
      steps {
        echo 'Checkout the source code from GitHub'
        git branch: 'master', url: 'https://github.com/Snehansh09-03/star-agile-insurance-project.git'
            }
    }
    
    stage('Package the Application') {
      steps {
        echo " Packaing the Application"
        sh 'mvn clean package'
            }
    }
    
        
    stage('Docker Image Creation') {
      steps {
        sh 'docker build -t shivansh8068/insurance_staragile:1.0 .'
            }
    }
    stage('Docker-Login') {
      steps {
         withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                                   }
                        }
                }

    stage('Push Image to DockerHub') {
      steps {
        sh 'docker push shivansh8068/insurance_staragile:1.0'
            }
    } 
    stage('Deploy Application using Ansible') {
      steps {
        
        ansiblePlaybook credentialsId: 'sshid', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'deploy.yml', vaultTmpPath: ''
            }
    }

  }
}
