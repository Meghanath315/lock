pipeline
{
    agent any
    parameters
    {
        choice (name: 'meghanath', choices: "dev\nqa", description: 'deploy')
    }
    stages
    {
        stage('git checkout')
        {
            steps
            {
               git 'https://github.com/Meghanath315/lock.git'
            }
            
        }
        stage('docker image build and push')
        {
            steps
            {
                
                withDockerRegistry(credentialsId: 'docker-cred', url: 'https://index.docker.io/v1/')
                {
                    sh 'docker build -t meghanath315/tomim1:1 -f Dockerfile .'
                    sh 'docker push meghanath315/tomim1:1'
                    
                                       
                }
            }
            
        }
        stage('playbook for dev')
        {
            steps
            {
                script
                {
                    if("${params.meghanath}" == "dev")
                {
                ansiblePlaybook credentialsId: 'ans-crd', installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/var/lib/jenkins/workspace/meg/ansible.yml', skippedTags: 'qa', tags: 'dev'
                }
                    
                }
                
            }
        }
        stage('playbook for qa')
        {
            steps
            {
                script
                {
                    if("${params.meghanath}" == "qa")
                {
                ansiblePlaybook credentialsId: 'ans-crd', installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/var/lib/jenkins/workspace/meg/ansible.yml', skippedTags: 'dev', tags: 'qa'
                }
                    
                }
                
            }
            
        }
        
    }
}
