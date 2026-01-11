pipeline{
    agent {label 'dev'}
    stages{
        stage('code checkout'){
            steps {
                 git barnch: 'master', url:'https://github.com/devvikasmanda/demo-project.git'

            }
        }
        stage ('maven build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage ('code coverage'){
            steps{
                sh 'mvn site'
            }
        }
    }
}
