pipeline{
    agent {label 'dev'}
    stages{
        stage('code checkout'){
            steps {
                 git branch: 'master', url:'https://github.com/devvikasmanda/demo-project.git'

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
        stage ('sonarqube code analysis'){
            steps{
                withSonarQubeEnv('my_sonar')
                {
                    // dir ("./server"){
                    //     sh 'mvn sonar:sonar'
                    // }
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}
