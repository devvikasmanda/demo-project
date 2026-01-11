pipeline{
    agent {label 'dev'}
    stages{
        stage('code checkout'){
            steps {
                 git 'https://github.com/devvikasmanda/demo-project.git'

            }
        }
        stage ('maven build'){
            steps{
                sh 'mvn clean install'
            }
        }
    }
}
