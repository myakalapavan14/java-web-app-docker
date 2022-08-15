node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.5.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t myakalapavan/java-web-app .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u myakalapavan -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push myakalapavan/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app myakalapavan/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.109.54.15docker stop java-web-app || true'
          sh 'ssh  ubuntu@3.109.54.15 docker rm java-web-app || true'
          sh 'ssh  ubuntu@3.109.54.15 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@3.109.54.15 ${dockerRun}"
       }
       
    }
     
     
}
