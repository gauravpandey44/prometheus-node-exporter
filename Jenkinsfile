pipeline 
{
    agent any
    environment 
    {
            REPO = sh(script: 'echo "${GIT_URL}" | sed -e "s/https:\\/\\/github.com\\/.*\\///" | sed "s/.git//"',returnStdout: true).trim()
    }
    stages 
    {
       stage('Run Build') 
       {
           parallel 
           {
               stage('Build on homeserver') 
               {
                   agent 
                   {
                         label "homeserver_production"
                   }
                   steps 
                   {

                        echo "homeserver_production"
                        sh(""" 
                        mkdir -p /home/${USER}/PRODUCTION/dockers/${REPO}
                        cp -rp ${WORKSPACE}/* /home/${USER}/PRODUCTION/dockers/${REPO}/
                        """)

                    }
                }
                stage('Build on oracle-cloud') 
                {
                   agent 
                   {
                         label "oracle-cloud_production"
                   }
                   steps 
                   {

                        echo "oracle-cloud_production"
                        sh(""" 
                        mkdir -p /home/${USER}/PRODUCTION/dockers/${REPO}
                        cp -rp ${WORKSPACE}/* /home/${USER}/PRODUCTION/dockers/${REPO}/
                        """)
                   }
                }
                stage('Build on oracle-cloud2') 
                {
                   agent 
                   {
                          label "oracle-cloud2_production"
                   }
                   steps 
                   {

                        echo "oracle-cloud2_production"
                        sh(""" 
                        mkdir -p /home/${USER}/PRODUCTION/dockers/${REPO}
                        cp -rp ${WORKSPACE}/* /home/${USER}/PRODUCTION/dockers/${REPO}/
                        """)

                   }
                }

            }
        }
        stage('Run Test') 
        {
            parallel 
            {
                stage('Test on homeserver') 
                {
                   agent 
                   {
                    label "homeserver_production"
                   }
                   steps 
                   {

                        sh(""" 
                        echo "TESTING"
                        """)

                    }
                }
                stage('Test on oracle-cloud') 
                {
                   agent 
                   {
                        label "oracle-cloud_production"
                   }
                   steps 
                   {

                        sh(""" 
                        echo "TESTING"
                        """)

                    }
                }
                stage('Test on oracle-cloud2') 
                {
                   agent 
                   {
                         label "oracle-cloud2_production"
                   }
                   steps 
                   {

                        sh(""" 
                        echo "TESTING"
                        """)

                    }
                }
            }
        }
        stage('Run Deploy') 
        {
            parallel 
            {
                stage('Deploy on homeserver') 
                {
                    agent 
                    {
                        label "homeserver_production"
                    }
                    steps 
                    {

                          sh("""
                          cd /home/${USER}/PRODUCTION/dockers/${REPO}/
                          docker-compose up -d
                          """)
                    }

                }
               stage('Deploy on oracle-cloud') 
               {
                    agent 
                    {
                        label "oracle-cloud_production"
                    }
                    steps 
                    {

                          sh("""
                          cd /home/${USER}/PRODUCTION/dockers/${REPO}/
                          docker-compose up -d
                          """)
                    }

                }
               stage('Deploy on oracle-cloud2') 
               {
                    agent 
                    {
                        label "oracle-cloud2_production"
                    }
                    steps 
                    {

                          sh("""
                          cd /home/${USER}/PRODUCTION/dockers/${REPO}/
                          docker-compose up -d
                          """)
                    }
                }
            }
        }
    }
    post 
    {
        always 
        {
            echo "Sending Email"
            emailext subject: '$DEFAULT_SUBJECT',
            body:  ''' 
                $DEFAULT_CONTENT
              ''',
            recipientProviders: [
                [$class: 'RequesterRecipientProvider']
            ], 
            replyTo: '$DEFAULT_REPLYTO',
            to: '$DEFAULT_RECIPIENTS',
            mimeType: 'text/html'
       }
    }
}  
     