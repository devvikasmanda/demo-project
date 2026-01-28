pipeline{
  agent any
  stages { 
        stage('code checkout') {
            steps {
              sh 'rm -rf *'
                git branch: 'master', url: 'https://github.com/devvikasmanda/demo-project.git'
            }
        }
    stage('maven build') {
            steps {
                sh 'mvn clean install'
            }
        }
  }
} 
