pipeline{
    agent {label 'dev'}
    stages{
        stage('code checkout'){
            steps{
                git branch: 'master' , url: 'https://github.com/devvikasmanda/demo-project.git'
            } 
        }
        stage('maven build'){
            steps {
                sh 'mvn clean install'
            }
        }
        stage('code compile'){
            steps{
                sh 'mvn site'
            }
        }
        stage('sonarqube code analysis') {
            steps {
                withSonarQubeEnv('vikas') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
        
}
