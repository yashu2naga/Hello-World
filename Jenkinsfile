node {
    //def mvnHome
    stage('Preparation') { // for display purposes
        // Get some code from a GitHub repository
        git 'https://github.com/yashu2naga/spring-boot-mongo-docker.git'
        // Get the Maven tool.
        // ** NOTE: This 'M3' Maven tool must be configured
        // **       in the global configuration.
        //mvnHome = tool 'M3'
    }
    stage("maven"){
        def mavenHome = tool name: "maven", type : "maven"
        def mavenCMD = "${mavenHome}/bin/mvn " 
        sh "${mavenCMD}  clean package"
        
    }
    stage("Build Docker Image"){
        sh "docker build -t  yasaswini06/spring-boot-mongo ."
    }
    stage("Docker push"){
        withCredentials([string(credentialsId: 'docker', variable: 'docker_cred')]) {
    // some block
        sh "docker login -u yasaswini06 -p ${docker_cred}"
    
       }    

        sh "docker push yasaswini06/spring-boot-mongo"
        
    }
    
     stage("Deploy Application in K8s cluster"){
         kubernetesdeploy(
             configs: 'springBootMongo.yml',
         kubeconfigID: 'kubernet_cred',
         enableconfigsubstitution: true
         )
     } 

}
    
