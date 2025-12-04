pipeline {
      agent  any 


   tools{
          maven 'maven-demo'
          jdk  'java17'
   }

    environment{
         
          MVN_CMD = "${tool 'maven-demo'}/bin/mvn"
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    } 

    stages{
         stage('CHeckout'){
               steps{
                   git  branch : 'main', url: 'https://github.com/Nikhil00-7/java-maven-demo-project-2.git'
               }
         }

          stage('Build'){
            steps{
                 sh "${MVN_CMD} clean package -DskipTests" 
             }
          }

          stage('Test'){
               steps{
                echo "Running  unit  Test"
                sh "${MVN_CMD} test"
            }
          }

          stage('Deploy'){
              steps{
                echo "Deploying  Application"
                    sh "java -jar target/*.jar &"
              }
          }
          stage('Integration Test'){
              steps{
                echo "Running Integration Test"
                sh "curl http://localhost:8080/api/greet"
              }
          }

          stage('CleanUp'){
              steps{
                 echo "Cleaning up the Application"
                 sh "pkill -f 'java -jar'"
              }
          }

           stage('Archive Artifacts'){
                 steps{
                      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                 }

           }

    }


post {
    success {
        echo "Pipeline is successful"
        mail(
            to: 'kujurnikhil0007@gmail.com',
            subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: "Good news! The build succeeded.\nCheck the details at ${env.BUILD_URL}"
        )
    }
    failure {
        echo "Pipeline failed"
        mail(
            to: 'kujurnikhil0007@gmail.com',
            subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: "Unfortunately, the build failed.\nCheck the details at ${env.BUILD_URL}"
        )
    }


   }
}
