pipeline 
{
  agent any

  tools
    {
        maven 'maven-3.6.3'
    }

  stages 
  {
    stage('Build') 
    {
      steps 
      {
        echo 'building the application'
        sh 'mvn build-helper:parse-version versions:set -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.nextMinorVersion}.\\\${parsedVersion.incrementalVersion}\\\${parsedVersion.qualifier?}' 
        sh 'mvn clean package'
        sh "docker build -t jeelkanani41/spring-boots ."
        sh "docker run -it -d -p 80:8080 jeelkanani41/spring-boots " 
      }
    }
    stage('Test') 
    {
      steps 
      {
        sh 'mvn test'
      }
    }
    stage('Deploy') 
    {
        steps 
        {
                script
                {
                echo 'deploying the application'
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')])
                {
                    sh "echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin"
                    sh "docker push jeelkanani41/spring-boot:${IMAGE_NAME}"
                }
                }
        }
    }
  }
}
