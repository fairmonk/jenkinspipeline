pipeline {
    agent {
        label "default"
    }
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.223.116.165', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.220.40.175', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

     stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

     stage('Deployments'){
         parallel{
            stage('Deploy to Staging') {
                steps{
                    bat "scp -i d:\\soft\\AWS\\tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                }
            }            
        }
        stage('Deploy to Production'){
            steps {
                bat "scp -i d:\\soft\\AWS\\tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps""
            }
        }
     }
    }
}