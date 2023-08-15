pipeline {
    agent {
        docker {
            image 'node:16' 
            args '-p 3000:3000' 
        }
    }
    stages {
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
        stage('Manual Approval'){
            steps{
                input message: 'Lanjutkan ke tahap Deploy?'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sshagent(credentials: ['ubuntu']) {
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.140.55.135 cd /home/ubuntu/a428-cicd-labs/ && git remote set-url origin /home/ubuntu/a428-cicd-labs && git pull origin react-app'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.140.55.135 sh /home/ubuntu/a428-cicd-labs/jenkins/scripts/deliver.sh'
                        sleep(time: 1, unit: 'MINUTES')
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.140.55.135 sh /home/ubuntu/a428-cicd-labs/jenkins/scripts/kill.sh'
                    }
                }
            }
        }
    }
}
/*sshagent(['mfajrizulfa']){
                //    sh '''ssh -o StrictHostChecking=no ubuntu@ec2-13-250-64-22.ap-southeast-1.compute.amazonaws.com'''
                //}
                sh './jenkins/scripts/deliver.sh'
                sleep(time: 1, unit: 'MINUTES')
                script {
  	              sshagent(credentials: ['ubuntu']) {
                    	sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.250.64.22 sh /home/ubuntu/a428-cicd-labs/jenkins/scripts/deliver.sh'
                    	sleep(time: 1, unit: 'MINUTES')
                    	sh 'ssh -o StrictHostKeyChecking=no ubuntu@13.250.64.22 sh /home/ubuntu/a428-cicd-labs/jenkins/scripts/kill.sh'
                	}
            	}
                sh './jenkins/scripts/deliver.sh'
                sleep(time: 1, unit: 'MINUTES')
                sh './jenkins/scripts/kill.sh'
                
                sh '''
                        sudo apt-get update
                        sudo apt-get install -y openssh-client
                    '''
                    sh 'ssh-agent bash -c "ssh-add /var/jenkins_home/.ssh/id_rsa; ssh -o StrictHostKeyChecking=no ubuntu@18.140.55.135 sh /home/ubuntu/a428-cicd-labs/jenkins/scripts/deliver.sh"'
                    sleep(time: 1, unit: 'MINUTES')
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@18.140.55.135 sh /home/ubuntu/a428-cicd-labs/jenkins/scripts/kill.sh'
                */