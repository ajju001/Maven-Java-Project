def ansible = [:]
         remote.name = 'ansible'
         remote.host = '172.31.30.154'
         remote.user = 'centos'
         remote.password = 'Rnstech@123'
         remote.allowAnyHosts = true
def kops = [:]
         remote.name = 'kops'
         remote.host = '172.31.34.51'
         remote.user = 'centos'
         remote.password = 'Rnstech@123'
         remote.allowAnyHosts = true
pipeline {
    agent { label 'buildserver'}

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven3.6"
    }

    stages {
        stage('Prepare-Workspace') {
            steps {
                // Get some code from a GitHub repository
                git credentialsId: 'github-server-credentials', url: 'https://github.com/venkat09docs/Maven-Java-Project.git'                
            }
            
        }
        stage('Tools-Setup') {
            steps {
                sshCommand remote: ansible, command: 'cd Maven-Java-Project; git pull'
                sshCommand remote: ansible, command: 'cd Maven-Java-Project; ansible-playbook -i hosts tools/sonarqube/sonar-install.yaml'
                sshCommand remote: ansible, command: 'cd Maven-Java-Project; ansible-playbook -i hosts tools/docker/docker-install.yml'   
                     
                //K8s Setup
                sshCommand remote: kops, command: "cd Maven-Java-Project; git pull"
	       sshCommand remote: kops, command: "kubectl apply -f Maven-Java-Project/k8s-code/staging/namespace/staging-ns.yml"
	       sshCommand remote: kops, command: "kubectl apply -f Maven-Java-Project/k8s-code/prod/namespace/prod-ns.yml"
            }            
        }
    }
}
