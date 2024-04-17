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
                
                 winscpPublisher {
                        // Configure the WinSCP Publisher
                        failOnError: true,
                        host: params.tomcat_stag,
                        port: 22,
                        username: 'admin',
                        password: '',
                        sourceFiles: '**/*.war',
                        remoteDirectory: '/'
                    }
            }
        }

       
    }


    
}
