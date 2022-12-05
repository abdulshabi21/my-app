node{
   stage('SCM Checkout'){
     git 'https://github.com/damodaranj/my-app.git'                           
   }
stage('Compile-Package'){
    
      def mvnHome =  tool name: 'maven3', type: 'maven'                      
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
stage('Build Docker Imager'){
   sh 'docker build -t abdulshabi21/myweb:0.0.2 .'
   }
stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerPassword')]) {
   sh "docker login -u abdulshabi21 -p ${dockerPassword}"
    }
   sh 'docker push abdulshabi21/myweb:0.0.2'
   }
stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest5'
	}catch(error){
		//  do nothing if there is an exception
	}
}
stage('Docker deployment'){
   sh 'docker run -d -p 8095:8080 --name tomcattest5 abdulshabi21/myweb:0.0.2' 
   }
stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
         }
stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 13.232.28.48:8083"
   sh "docker tag abdulshabi21/myweb:0.0.2 13.232.28.48:8083/shabi:1.0.0"
   sh 'docker push 13.232.28.48:8083/shabi:1.0.0'
   }
}
