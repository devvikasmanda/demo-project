pipeline{
  agent any
  stages { 
        stage('code checkout') {
            steps {
              
                git branch: '3rd', url: 'https://github.com/devvikasmanda/demo-project.git'
            }
        }
    stage('maven build') {
            steps {
                sh 'mvn clean install'
            }
        }
    stage('sonarqube code analysis') {
            steps {
                withSonarQubeEnv('my_sonar1') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
  }
} 
