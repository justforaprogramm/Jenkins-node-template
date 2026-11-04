pipeline {
    agent any // Hier kannst du bei Bedarf einen spezifischen Node eintragen, auf dem Docker läuft

    environment {
        // Name und Tag für dein Image
        IMAGE_NAME = "custom-jenkins-agent"
        IMAGE_TAG  = "latest"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                // Das Image wird gebaut. Eine .env Datei wird hierfür weder benötigt noch verwendet.
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Test: Start & Stop') {
            steps {
                script {
                    // Container im Hintergrund (-d) starten. 
                    // --rm sorgt dafür, dass er nach dem Stoppen sofort gelöscht wird.
                    // Wir geben Dummy-Werte mit, damit der Entrypoint nicht sofort mit einem Fehler abbricht.
                    def containerId = sh(
                        script: """
                            docker run -d --rm \
                            -e JENKINS_URL=http://dummy \
                            -e JENKINS_SECRET=dummy \
                            -e JENKINS_AGENT_NAME=dummy \
                            ${IMAGE_NAME}:${IMAGE_TAG}
                        """, 
                        returnStdout: true
                    ).trim()

                    echo "Container erfolgreich gestartet mit ID: ${containerId}"

                    // 5 Sekunden warten, um zu prüfen, ob er anbleibt und nicht sofort abstürzt
                    sleep time: 5, unit: 'SECONDS'

                    // Container wieder stoppen (wird durch --rm automatisch restlos entfernt)
                    echo "Stoppe den Container..."
                    sh "docker stop ${containerId}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline beendet. Der Workspace ist sauber."
            // Optional: Wenn du das Image nach dem Build auch löschen willst, entkommentiere die nächste Zeile:
            // sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
        }
    }
}