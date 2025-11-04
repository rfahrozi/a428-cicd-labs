```groovy
// Jenkinsfile - Scripted Pipeline untuk React App
// Submission CI/CD Pipeline dengan Jenkins

node {
    def nodeImage = 'node:lts-buster-slim'
    def containerName = 'react-app-container'
    def appPort = '3000'
    
    try {
        // Stage 1: Checkout
        stage('Checkout') {
            echo '========================================='
            echo 'Stage: Checkout Source Code'
            echo '========================================='
            
            checkout scm
            
            echo 'Source code checked out successfully'
            sh 'ls -la'
        }
        
        // Stage 2: Build
        stage('Build') {
            echo '========================================='
            echo 'Stage: Build React Application'
            echo '========================================='
            
            docker.image(nodeImage).inside("--name ${containerName}-build") {
                echo 'Installing dependencies...'
                sh 'npm install'
                
                echo 'Building React application...'
                sh 'npm run build'
                
                echo 'Build completed successfully'
                sh 'ls -la build/'
            }
        }
        
        // Stage 3: Test
        stage('Test') {
            echo '========================================='
            echo 'Stage: Test React Application'
            echo '========================================='
            
            docker.image(nodeImage).inside("--name ${containerName}-test") {
                echo 'Running tests...'
                
                sh 'npm install'
                
                sh 'CI=true npm test'
                
                echo 'Tests completed successfully'
            }
        }
        
        // Stage 4: Archive Artifacts
        stage('Archive') {
            echo '========================================='
            echo 'Stage: Archive Build Artifacts'
            echo '========================================='
            
            archiveArtifacts artifacts: 'build/**/*', fingerprint: true
            
            archiveArtifacts artifacts: 'package.json', fingerprint: true
            
            echo 'Artifacts archived successfully'
        }
        
        echo '========================================='
        echo 'Pipeline completed successfully! ✓'
        echo '========================================='
        currentBuild.result = 'SUCCESS'
        
    } catch (Exception e) {
        echo '========================================='
        echo 'Pipeline failed! ✗'
        echo "Error: ${e.getMessage()}"
        echo '========================================='
        currentBuild.result = 'FAILURE'
        throw e
        
    } finally {
        stage('Cleanup') {
            echo '========================================='
            echo 'Stage: Cleanup'
            echo '========================================='
            
            echo 'Cleaning up workspace...'
            
            sh """
                docker ps -a | grep ${containerName} | awk '{print \$1}' | xargs -r docker rm -f || true
            """
            
            echo 'Cleanup completed'
        }
        
        echo "Build Status: ${currentBuild.result}"
    }
}
