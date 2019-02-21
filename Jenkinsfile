pipeline {
    agent any
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '113.16.409.108', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '132.23.27.36', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war' ## Mention full path here "C:\Program Files (x86)\Jenkins\workspace\maven-project\webapp\target" - this is in windows
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /c/Users/ybk/code/da.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /c/Users/ybk/code/da.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
