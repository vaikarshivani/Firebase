
pipeline {
    agent any
    
    stages {
        stage('Checkout code') {
            steps {
                checkout scm
            }
        }
        
        stage('Set up JDK') {
            steps {
                tool name: 'JDK 17', type: 'jdk'
            }
        }
        
        stage('Build with Maven') {
            steps {
                sh 'mvn -B clean install'
            }
        }
        
        stage('Store artifact') {
            steps {
                sh 'mkdir -p artifacts'
                sh 'cp token/target/*.jar artifacts/'
            }
        }
        
        stage('Commit and push artifact') {
            steps {
                script {
                    def gitActor = env.GITHUB_ACTOR
                    gitUserName(gitActor)
                    gitUserEmail("${gitActor}@users.noreply.github.com")
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'YOUR_CREDENTIALS_ID', url: 'YOUR_GITHUB_REPO_URL']]])
                    sh 'git add artifacts/'
                    sh 'git commit -m "Add built artifact"'
                    sh 'git push origin HEAD:refs/heads/main'
                }
            }
        }
    }
}
