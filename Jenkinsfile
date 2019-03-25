env.dockerimagename="devopsbasservice/buildonframework:buildon-v1.0"
node {
  stage ('Banking_Checkout') {
   //If some other Repository is to be given apart from current repo, provide git URL as below.
    checkout scm
  }
   stage ('Banking_Build') {
    sh 'mvn clean package -DskipTests=True'
  }
  stage ('Banking_UnitTest') {
    sh 'mvn test'
  }
   stage ('Banking_CodeAnalysis') {
    sh 'mvn sonar:sonar -Dmaven.test.failure.ignore=true -DskipTests=true -Dsonar.sources=src/main/java'
   }

   stage ('Banking_NexusUpload') {
   
          sh "mvn -B help:evaluate -Dexpression=project.groupId | grep -e '^[^[]' > groupIdFile"
          groupId=readFile('groupIdFile').trim()
          
          sh "mvn -B help:evaluate -Dexpression=project.artifactId | grep -e '^[^[]' > artifactIdFile"
          artifactId=readFile('artifactIdFile').trim()
          
          sh "mvn -B help:evaluate -Dexpression=project.version | grep -e '^[^[]' > versionFile"
          version=readFile('versionFile').trim()
       
          sh "mvn -B help:evaluate -Dexpression=project.build.finalName | grep -e '^[^[]' > finalNameFile"
          projectName=readFile('finalNameFile').trim()

          sh "mvn -B help:evaluate -Dexpression=project.packaging | grep -e '^[^[]' > packagingFile"
          packaging=readFile('packagingFile').trim()
          
          artifactName="target/${projectName}.${packaging}"
          
          sh "mvn deploy:deploy-file -DgroupId=${groupId} -DartifactId=${artifactId} -Dpackaging=${packaging} -Dversion=${version} -DgeneratePom=true -Dfile=${artifactName}"
   }
  
   stage ('Banking_tomcatDeployment') {
    sh 'mvn -P tomcatDeployment tomcat7:redeploy -DskipTests'
   }   
}    
