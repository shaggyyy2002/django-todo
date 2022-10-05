node {
    stage('git checkout'){
        git 'https://github.com/shaggyyy2002/django-todo.git'
    }
    stage('docker build image'){
        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID . '
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID nitin03/$JOB_NAME:v1.$BUILD_ID'
        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID nitin03/$JOB_NAME:latest'
    }
    stage('pushing images to DockerHub'){
        withCredentials([string(credentialsId: 'DockerHubPassword1', variable: 'dockerhubPassword')]) {
            sh "docker login -u nitin03 -p ${dockerhubPassword}"
            sh 'docker image push nitin03/$JOB_NAME:v1.$BUILD_ID'
            sh 'docker image push nitin03/$JOB_NAME:latest'
            
            sh 'docker image rm $JOB_NAME:v1.$BUILD_ID nitin03/$JOB_NAME:v1.$BUILD_ID nitin03/$JOB_NAME:latest'
        }
    stage('Docker conatiner Deplyment'){
        sshagent(['webapp-server']) {
            def docker_run = 'docker run -p 9000:80 -d --name scriptedconatiner nitin03/docker-webapp'
            def docker_rmv_container = 'docker rm -f scriptedconatiner'
            def docker_rmi = 'docker rmi -f nitin03/docker-webapp'
            
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.34.27 ${docker_rmv_container}"
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.34.27 ${docker_rmi}"
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.34.27 ${docker_run}"
            }
    }    
    }
}
