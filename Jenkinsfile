pipeline {
    agent any
    environment {
        DOCKER_IMAGE    = "vikasmanda/final-project-app"
        DOCKER_TAG      = "${BUILD_NUMBER}"
    }

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
        stage('docker build'){
            steps{
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG} ${DOCKER_IMAGE}:latest"

            }
        }
        stage('Docker Push') {
            steps {
                // 'docker-hub-credentials' must be created in Jenkins Credentials
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', 
                                                 passwordVariable: 'DOCKER_PASS', 
                                                 usernameVariable: 'DOCKER_USER')]) {
                    sh "echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

    }
}
