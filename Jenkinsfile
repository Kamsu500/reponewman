pipeline {
    agent { 
         docker { 
            image 'postman/newman:alpine'
            args '-u root'
        }
    }

    stages {
        stage('Run Newman Tests') {
            steps {
                sh 'newman run jenkinsnewman/posts_e2e.postman_collection.json ' +
                   '--reporters html,junit ' +
                   '--reporter-html-export newman-reports/html-report.html ' +
                   '--reporter-junit-export newman-reports/junit-report.xml'
            }
        }
    }

    post {
        always {
            sh 'echo "Affichage des rapports de collection newman"'
            archiveArtifacts artifacts: 'cypress/reports/**/*.html', allowEmptyArchive: true
        }
    }
}
