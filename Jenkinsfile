pipeline {
   agent {label "master"}
    
    tools {
        maven 'maven'
        jdk '1.8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

       
      
      stage ('Compile-Package') {
            steps {
                git "https://github.com/javahometech/my-app"
              sh 'mvn clean package'
            }
          post {
                success {
                    archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
                    junit '**/target/surefire-reports/*.xml' 
                }
            }
      }
  
      //stage('SonarQube analysis') {
         //Ws(/var/lib/jenkins)
    // requires SonarQube Scanner 2.4+
        // steps {
        // def scannerHome = tool 'SonarQube Scanner 2.4';
     
        //withSonarQubeEnv('My SonarQube Server') {
     //sh "${scannerHome}/opt/sonar/sonar-scanner"
    //}
 // }
 //}
       
   stage('Deploy to Tomcat'){
  steps {
  sshagent(['3d0ff4fe-87e0-468b-9c6f-fbd6f291a57b']) {
    sh "scp  /var/lib/jenkins/workspace/tomcat-from-slave-to-app/target/*war ubuntu@18.234.76.16:/opt/tomcat/apache-tomcat-8.5.37/webapps"
    
    }
    }
    }
  
       stage ('Email Notification'){
          steps{
      emailext (
    subject: "Job '${env.JOB_NAME} ${env.BUILD_NUMBER}'",
    body: """<p>Check console output at <a href="${env.BUILD_URL}">${env.JOB_NAME}</a></p>""",      
    to: "leghari.quratulain@gmail.com",
    from: "leghari.quratulain@gmail.com"
)
          }
       }
      }
   }


