pipeline {
    agent any

    tools{
        maven 'local_mvn'
    }

    parameters{
        string(name: 'tomcat_dev', defaultValue: '18.221.148.103', description: 'Staging Server')
        string(name: 'tomcat_prod', defaultValue: '18.188.72.6', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

     stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo '开始存档...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

     stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/gaoyan/Documents/SunnyDemo/dev/tomcat-demo.pem.txt **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/gaoyan/Documents/SunnyDemo/dev/tomcat-demo.pem.txt **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}