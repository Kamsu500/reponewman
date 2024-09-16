pipeline {
    agent { 
         docker { 
            image 'postman/newman:alpine'
            args '-u root'
        }
    }

    stages {
        stage('Installation de newman') {
            steps {
                sh 'npm install -g newman newman-reporter-htmlextra'
            }
        }
        stage('Run Newman tests') {
            steps {
                sh '''
                 mkdir -p reports
                 newman run posts_e2e.postman_collection.json \
                 --reporters cli,json,junit,htmlextra \
                 --reporter-htmlextra-export ./reports/myreport.html \
                 --reporter-htmlextra-theme default
                 
                 // Vérifier le contenu du répertoire reports
                 ls -la ./reports
                 
                 // Afficher le contenu du rapport HTML
                 cat ./reports/myreport.html
                '''
            }
        }
    }

    post {
        always {
            // Archiver les rapports générés
            archiveArtifacts artifacts: 'reports/*.html', allowEmptyArchive: true

            // Publier le rapport HTML
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'reports',
                reportFiles: 'myreport.html',
                reportName: 'My Reports',
                reportTitles: 'The Report'
            ])
        }
    }
}
