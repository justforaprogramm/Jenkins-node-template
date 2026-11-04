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
        stage('Build & Push Multi-Arch (AMD64 & ARM64)') {
            steps {
                echo "Logge bei ghcr.io ein..."
                // Login bei GitHub Container Registry
                sh "echo \$GH_TOKEN | docker login ghcr.io -u justforaprogramm --password-stdin"
                
                echo "Baue das Image für AMD64 & ARM64 und pushe es direkt..."
                // Ein dedizierter Builder wird erstellt, genutzt und wieder entfernt, um Konflikte zu vermeiden
                sh """
                    docker buildx create --use --name multi-builder || true
                    docker buildx build --platform linux/amd64,linux/arm64 -t ${IMAGE_NAME}:${IMAGE_TAG} --push .
                    docker buildx rm multi-builder || true
                """
            }
        }
    }

    post {
        always {
            echo "Räume auf: Logge aus Registry aus..."
            // Zur Sicherheit wieder ausloggen
            sh "docker logout ghcr.io || true"
        }
    }
}