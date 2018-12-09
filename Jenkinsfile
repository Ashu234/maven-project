pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '34.238.152.61', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.90.177.54', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "winscp -i C:\Users\Kharayat\Desktop\Jenkins\MyTomcatDemoKey.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i C:\Users\Kharayat\Desktop\Jenkins\MyTomcatDemoKey.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}