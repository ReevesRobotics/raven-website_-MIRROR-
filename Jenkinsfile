node {
    def app

    stage('clone') {
        checkout scm
    }

    stage('build') {
        app = docker.build("jenkins/rrweb_dev:${env.BUILD_ID}")
    }

    stage('push') {
        docker.withRegistry('https://gitea.caffeinatedope.com/', 'gitea') {
            app.push();
        }
    }
    stage('kill prev') {
        try {
            sh 'docker kill rrweb_dev'
        } catch(err) {
            echo "container not running"
        }
    }
    stage('run') {
        docker.withRegistry('https://gitea.caffeinatedope.com/') {
            docker.image("jenkins/rrweb_dev:${env.BUILD_ID}").run("-p 5173:5173 --name rrweb-dev -d --rm")
        }
    }
}