pipeline {
  
    agent any
  
     environment {
            REPO = sh(script: 'echo "${GIT_URL}" | sed -e "s/https:\\/\\/github.com\\/.*\\///" | sed "s/.git//"',returnStdout: true).trim()
     }

    stages {
       stage('Run Build') {
           parallel {
               stage('Build on homeserver') {
                   agent {
                    label "homeserver_production"
                    }
                   steps {

                        echo "homeserver_production"
                        sh(""" 
                        mkdir -p /home/${USER}/PRODUCTION/dockers/${REPO}
                        cp -rp ${WORKSPACE}/* /home/${USER}/PRODUCTION/dockers/${REPO}/
                        """)

                

                    }
                }
                 stage('Build on oracle-cloud') {
                   agent {
                    label "oracle-cloud_production"
                   }
                   steps {

                        echo "oracle-cloud_production"
                        sh(""" 
                        mkdir -p /home/${USER}/PRODUCTION/dockers/${REPO}
                        cp -rp ${WORKSPACE}/* /home/${USER}/PRODUCTION/dockers/${REPO}/
                        """)
                      }
                }
                 stage('Build on oracle-cloud2') {
                   agent {
                    label "oracle-cloud2_production"
                   }
                   steps {

                        echo "oracle-cloud2_production"
                        sh(""" 
                        mkdir -p /home/${USER}/PRODUCTION/dockers/${REPO}
                        cp -rp ${WORKSPACE}/* /home/${USER}/PRODUCTION/dockers/${REPO}/
                        """)

                    }
                }

            }
        }
    }
}  
     