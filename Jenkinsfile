pipeline {
    agent any
    tools {
        // Specify the JDK to use by its configured name
        jdk 'jdk-17'
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_stag', defaultValue: '35.154.81.229', description: 'Tomcat Staging Server')
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Use the 'package' phase to build and package the application
                    sh 'mvn clean package'
                }
            }
            post {
                success {
                    echo 'Archiving the artifacts'
                    archiveArtifacts artifacts: '**/*.war'
                }
                
                }
               }
          stage('Deploy') {
            steps {
               script {
                    String tomcatUrl = 'http://localhost:8080';
                    String tomcatManagerUrl = tomcatUrl + '/manager/text';
                    String tomcatUsername = 'admin';
                    String tomcatPassword = 'admin';
                    String contextName = '/LoginWebApp'; // Change this to your desired context path

                    sh "curl -v --user ${tomcatUsername}:${tomcatPassword} --upload-file target/LoginWebApp.war ${tomcatManagerUrl}/deploy?path=${contextName}"
                }
            }
        }
        

       
    }


    
}
