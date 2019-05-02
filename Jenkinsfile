pipeline {
    agent {
        node {
            label 'docker-io-ui'
        }
    }
      environment {
        jenkins_openshift_username="c8fdb02e-71cf-4907-963d-19203ee74bb0"
        jenkins_openshift_password="68ec65fb-b20e-4f37-bf26-191bb85d6757"
        openshift_applier_url="f3915c69-b13e-42c2-b8d8-325cfbcbbfe9"
      }

    options { skipDefaultCheckout() }

    stages {
        stage ('Initialize Application Migration') {
            agent none
            steps {
                script {
                        withCredentials(
                                        [string(credentialsId: jenkins_openshift_username, variable: 'OPENSHIFT_ID'),
                                        string(credentialsId: jenkins_openshift_password, variable: 'OPENSHIFT_PASS'),
                                        string(credentialsId: openshift_applier_url, variable: 'OPENSHIFT_APPLIER_URL')])
                           {
                        
  
                                stage ('Ansible Play') {
                                        retry(3) {
                                                sh 'oc login https://openshiftnextgen.in.dst.ibm.com:8443 -u kumar -p kumar --insecure-skip-tls-verify=true'
                                        }
                                        sh 'rm -rf ansible_playbook_templates_java'
                                        echo 'Cloning repository'
                                        sh 'git clone https://ntooling:3bf14bb1d09b4fa7fe9a49ee601238bd72c23cab@github.ibm.com/ngt/ansible_playbook_templates_java.git'
                                        echo 'Install ansible galaxy'
                                        sh 'ansible-galaxy install -r ansible_playbook_templates_java/requirements.yml --roles-path=ansible_playbook_templates_java/galaxy'
                                        echo 'Applying Ansible playbooks for docker build and push'
                                        sh 'ansible-playbook -i ansible_playbook_templates_java/.applier/ ansible_playbook_templates_java/galaxy/openshift-applier/playbooks/openshift-cluster-seed.yml'
                                }
                         

                        }
                }
            }
        }
    }
}
