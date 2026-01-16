pipeline {
    agent any
    stages { 
        stage('code checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/devvikasmanda/demo-project.git'
            }
        }
        stage('maven build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('code coverage') {
            steps {
                sh 'mvn site'
            }
        }
        stage('sonarqube code analysis') {
            steps {
                withSonarQubeEnv('my_sonar1') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('quality gates') {
            steps {
                script {
                    // This waits for SonarQube to report back the status
                    timeout(time: 1, unit: 'MINUTES') {
                        def check = waitForQualityGate()
                        if (check.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${check.status}"
                        }
                    }
                }
            }
        }
        stage('Artifactory_upload') {
            steps {
                script {
                    def server = rtServer(id: 'jfrog')
                    dir('./server/target/') {
                        rtUpload(
                            serverId: 'jfrog',
                            spec: '''{
                                "files": [{
                                    "pattern": "*.jar",
                                    "target": "vikas/"
                                }]
                            }'''
                        )
                    }
                }
            }
        }
    }
}
