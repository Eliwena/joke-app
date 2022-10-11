pipeline {
    agent any

    environment {
        VERSION = "latest"
        registry = "registry.heroku.com/joke-jenkins-eli/web"
    }

    stages {
        stage('build-test') {
            steps {
                script{
                    // bat "npm install -g pnpm"
                    bat "npm install"
                    bat "npm run build"
                    bat "npm test"
                }
            }
        }

    stage('deploy') {
        steps {
            script {
                VERSION = bat([script: "node -e \"console.log(require(\'./package.json\').version)\"", returnStdout: true]).trim()
                docker.withRegistry('https://registry.heroku.com', 'herokuId') {
                bat "docker buildx build --platform linux/amd64 -t ${registry}:$VERSION ."
                bat "docker push ${registry}:$VERSION"
                }
            }

            withCredentials([usernamePassword(credentialsId: 'herokuId', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                // sh "echo ${USERNAME}  echo ${PASSWORD} | heroku login"
                bat "HEROKU_API_KEY=${PASSWORD} npx heroku container:release web --app=joke-jenkins-eli"
            }
        }
    }
    }
}