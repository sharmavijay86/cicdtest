pipeline {
  agent any
  stages {
    stage('code checkout') {
      steps {
        git(url: 'https://github.com/sharmavijay86/cicdtest', branch: 'main', credentialsId: 'git', poll: true)
      }
    }

  }
}