node {
   def mvnHome
   def app
   stage('Preparation') { 
      git 'https://github.com/NeerajAgarwal7/Devops-201-maven.git'
      mvnHome = tool 'M3'
   }
   
   stage('App Build') {
       sh "echo ${BUILD_NUMBER}"
       sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
   }
  
  stage('Docker Image Build') {
      app = docker.build("neerajagarwal7/devops-201:1-${BUILD_NUMBER}")
     }
  
  stage('Push Docker Image') {
      docker.withRegistry('https://registry.hub.docker.com','docker-credentials') {
        app.push("${BUILD_NUMBER}")
        app.push("latest")
      }
  }
  
  stage('Run Container') {
      sh "sudo docker run -p 8082:8080 -d neerajagarwal7/devops-201"
  }
}
