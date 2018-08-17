node {
   def mvnHome
   stage('Prepare') {
      git url: 'git@github.com:nandudemy/devops.git', branch: 'develop'
      mvnHome = tool 'maven'
   }
   stage('Build') {
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('Unit Test') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Integration Test') {
     if (isUnix()) {
        sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean verify"
     } else {
        bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean verify/)
     }
   }
   if(env.BRANCH_NAME == 'develop'){
     stage("Upload"){
        // Artifact repository upload steps here
        echo "uploaded to artifactory"
     }
     stage("Deploy"){
        // Deploy steps here
        echo "deployed to dev"
     }
   }
   if(env.BRANCH_NAME ==~ /release.*/){
     stage("Version"){
        // Artifact repository upload steps here
        echo "versioned the package"
     }
     stage("Promote"){
        // Deploy steps here
        echo "promoted to qa"
     }
   }
   
}
