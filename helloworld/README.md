# Jenkins Pipeline code

This is the first Jenkins pipeline project to build a simple Go application and use ansible playbook to deploy to Kubernetes Cluster. Techs includes: Jenkins Pipeline + GitHub + Docker + Docker Hub + Ansible + Kubernetes + Golang

Prerequisites:
1. You have Kubernetes cluster available for use
2. You have Jenkins installed and running
3. You have Docker CE installed and running on Jenkins server, or Docker Desktop if you running Jenkins on your Mac
   If you have not installed yet, you will have chance to install on step 8

Steps:

1. Prepare Go application code/files, main.go and main_test.go. main.go is for build stage, main_test.go is for test stage

2. Prepare a multistage Dockerfile to build Go application in a static executable from scratch

3. Prepare Kubernetes service and deployment yaml files, K8s_service.yml and K8s_deployment.yml

4. Prepare Ansible playbook to use k8s module to deploy Kubernetes objects and resource

5. Setup two credentials on Jenkins, one for GitHub and another one for Docker Hub
   5.1) Jenkins -> Credentials -> System -> Global credentials (unrestricted) -> Add Credentials
   5.2) Type/Kind choose "Username with password", and give a meaningful ID for each, like "GitHub" and "DockerHub"

6. Install plugins on Jenkins:
   git
   pipeline
   CloudBees Docker Build and Publish
   GitHub

7. Setup Jenkins user on Jenkins server to connect to Kubernetes Cluster:
   $ sudo mkdir -p ~jenkins/.kube
   $ sudo cp ~/.kube/config ~jenkins/.kube/
   $ sudo chown -R jenkins:jenkins ~jenkins/.kube/

8. Install Docker CE, Ansible and Openshift using pip3, openshift is for K8s module in Ansible
   $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   $ sudo yum install docker-ce docker-ce-cli containerd.io
   $ sudo pip3 install ansible
   $ sudo pip3 install openshift

9. Create Jenkinsfile

10. Setup a Jenkins Pipeline Job
    New Item -> input item name like "Hello-World", choose type as Pipeline, 
             -> use the Poll SCM as the build trigger
             -> set Script Path as "helloworld/Jenkinsfile" since I put all the necessary files under folder helloworld in  Jenkins_Lab repo, please set yours accordingly
             -> Pipeline -> Definition: choose "Pipeline script from SCM"
                         -> SCM: choose Git
                         -> Repository URL: https://github.com/jefff-guu/Jenkins_Lab.git
                         -> Credentials: choose the credentials created previously for GitHub
                         -> Branch Specifier: I use "*/dev" here, please change accordingly for your own, like "*/master"

11. Git push to dev branch in GitHub