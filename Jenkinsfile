pipeline {
    agent any

    environment {
        VERSION = "latest"
        registry = "registry.heroku.com/joke-jenkins-eli/web"
    }
    stages {
        stage('build') {
            steps {
                sh "npm install -g pnpm"
                sh "pnpm install"
                sh "pnpm build"
                sh "pnpm test"
            }
        }
        stage('deploy'){
            sh "export VERSION=\$(node -e \"console.log(require('package.json').version)\")"
            script{
                docker.withRegistry('https://registry.heroku.com','herokuId'){
                    sh "docker buildx build --platform linux/amd64 -t ${registry}:${VERSION}"
                    sh "docker push ${registry}:${VERSION}"
                }
            }
        }
    }
}