node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
        checkout scm
    }

    stage('Copy file for docker build') {
        sh "cp cleaner.sh build_container/cleaner.sh"
        sh "cp cron_script.sh build_container/cron_script.sh"
        sh "cp backup.go build_container/backup.go"
    }

    stage('Build image') {
        app = docker.build("nutellinoit/sidecar-backup-volumes","--pull build_container/")
    }


    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("latest")
            app.push("${env.BUILD_NUMBER}")
        }
    }

    stage('Build image Beekup') {
            app = docker.build("beeckup/sidecar-backup-volumes","--pull build_container/")
    }


    stage('Push image Beekup') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("latest")
            app.push("${env.BUILD_NUMBER}")
        }
    }
}
