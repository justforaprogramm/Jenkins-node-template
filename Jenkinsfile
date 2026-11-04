pipeline {
    agent any // Führt die Pipeline auf einem verfügbaren Jenkins-Node aus

    environment {
        // Der Name MUSS dieses Format haben: ghcr.io/<github-name>/<repo-name>
        IMAGE_NAME = "ghcr.io/justforaprogramm/jenkins-node-template"
        IMAGE_TAG  = "latest"
        
        // Lädt den GitHub-Token aus dem Jenkins Credential Store
        GH_TOKEN   = credentials('github-ghcr-token') 
    }

    stages {
        stage('Build Docker Image') {
            steps {
                echo "Baue das Image ${IMAGE_NAME}:${IMAGE_TAG}..."
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to GitHub Registry') {
            steps {
                echo "Logge bei ghcr.io ein und pushe das Image..."
                // Login bei GitHub Container Registry
                sh "echo \$GH_TOKEN | docker login ghcr.io -u justforaprogramm --password-stdin"
                
                // Image hochladen
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        always {
            echo "Räume auf: Lösche das lokale Image..."
            // Löscht das gerade gebaute Image vom Host (|| true verhindert einen Abbruch, falls das Image gar nicht erst gebaut wurde)
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
            
            // Zur Sicherheit auch wieder ausloggen
            sh "docker logout ghcr.io || true"
        }
    }
}