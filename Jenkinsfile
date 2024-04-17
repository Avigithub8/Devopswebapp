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

        stage('Deployments') {
            stage('Deploy to Staging Server') {
                steps {
                    script {
                        sh "scp **/*.war jenkins@${params.tomcat_stag}:\C:\Program Files\Apache Software Foundation\Tomcat 9.0_ApTomcat9\webapps"
                    }
                }
            }
        }
    }


    
}
