pipeline {
    agent any

    stages {
        stage('slack notify') {
            steps {
                echo 'Hello slack '
                sh 'sleep 20'
            }
        }
        stage('scm checkout') {
            steps {
              git branch: 'main', credentialsId: 'git', url: 'https://github.com/sharmavijay86/cicdtest'
              sh 'sleep 20'
            }
        }
        stage('modify file') {
            steps {
                parallel (
                    dryrun: {
                        sh "echo 'Thread 1 running with option' "
                        sh 'sleep 20'
                    },
                    file: {
                        sh 'echo "Hello this is my ${BUILD_NUMBER} " > hello.txt'
                        sh 'ls && cat hello.txt' 
                        sh 'sleep 20'
                    }
                )
            }
        }
        stage('push') {
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
                       
    }
      post {
        always {
            cleanWs()
        }
      }
}
