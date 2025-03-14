pipeline {
    agent any
    stages {
        stage('Build Maven') {
            steps {
                git url: 'https://github.com/Vishwabharathy333/cicdvishwa/', branch: "master"
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t vishwabharathy/endtoendproject25may:v1 .'
                }
            }
        }
        stage('Docker login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push vishwabharathy/endtoendproject25may:v1"
                }
            }
        }
        stage('Deploy to k8s') {
            when { expression { env.GIT_BRANCH == 'master' } }
            steps {
                script {
                    kubernetesDeploy(configs: 'deploymentservice.yaml', kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
