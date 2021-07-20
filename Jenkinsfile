pipeline {
    agent any

    stages {
        stage('slack notify') {
            steps {
                echo 'Hello slack '
                sh 'sleep 10'
            }
        }
        stage('scm checkout') {
            steps {
              git branch: 'main', credentialsId: 'git', url: 'https://github.com/sharmavijay86/cicdtest'
              sh 'sleep 20'
            }
        }
        stage('code check') {
            steps {
                echo 'code check'
                sh 'sleep 10'
            }
        }
        stage('code scan sonarqube') {
            steps {
                echo 'code scan '
                sh 'sleep 20'
            }
        }
        stage('Docker image build and push') {
            steps {
                echo 'docker image build'
                sh 'sleep 20'
            }
        }
        stage('anchore image scan') {
            steps {
                echo 'image scan'
                sh 'sleep 10'
            }
        }

        stage('modify manifest') {
            steps {
                parallel (
                    dryrun: {
                        sh "echo 'Thread 1 running with option' "
                        sh 'sleep 20'
                    },
                    file: {
                        sh 'echo "Hello this is docker image ${BUILD_NUMBER} " > hello.txt'
                        sh 'sleep 20'
                    }
                )
            }
        }
        stage('push new image manifest') {
            steps {
 withCredentials([usernamePassword(credentialsId: 'git', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                         script {
                        env.encodedPass=URLEncoder.encode(PASS, "UTF-8")
                    }
                    sh """
                    git add --all
                    git commit -m "foobar" 
                    git push https://${USER}:${encodedPass}@github.com/sharmavijay86/cicdtest.git 
                    """
                    }
                echo "stage push"
                
                } 
        } 
        stage('deploy on k8s') {
            steps {
              sh 'echo deploy on kubernetes cluster'
              sh 'sleep 20'
            }
        }                  
    }

      post {
        always {
            cleanWs()
        }
      }
}
