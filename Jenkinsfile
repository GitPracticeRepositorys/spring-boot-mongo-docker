node{
     
    stage('SCM Checkout'){
        git credentialsId: 'githubcredentials', url: 'https://github.com/GitPracticeRepositorys/spring-boot-mongo-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t shivakrishna99/spring-boot-mongo .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOCKER_HUB_CREDENTIALS', variable: 'DOKCER_HUB_CREDENTIALS')]) {
          sh "docker login -u shivakrishna99 -p ${DOKCER_HUB_CREDENTIALS}"
        }
        sh 'docker push shivakrishna99/spring-boot-mongo'
     }
     
	 
     stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'KubeConfig',
         enableConfigSubstitution: true
        )
     }  
	 
	 /** 
      stage("Deploy To Kuberates Cluster"){      
        sh 'kubectl apply -f springBootMongo.yml'
      } **/
     
}


