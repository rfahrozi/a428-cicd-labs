
node {
    // Define variables
    def nodeImage = 'node:16-buster-slim'  // Changed to Node 16 for compatibility
    def containerName = 'react-app-container'
    def appPort = '3000'
    
    try {
        // Stage 1: Checkout
        stage('Checkout') {
            echo '========================================='
            echo 'Stage: Checkout Source Code'
            echo '========================================='
            
            // Checkout code from Git repository
            checkout scm
            
            echo 'Source code checked out successfully'
            sh 'ls -la'
        }
        
        // Stage 2: Build
        stage('Build') {
            echo '========================================='
            echo 'Stage: Build React Application'
            echo '========================================='
            
            // Use Docker agent to build the application
            docker.image(nodeImage).inside("--name ${containerName}-build") {
                echo 'Installing dependencies...'
                sh 'npm install'
                
                echo 'Building React application...'
                // Set NODE_OPTIONS to use legacy OpenSSL provider for compatibility
                sh 'NODE_OPTIONS=--openssl-legacy-provider npm run build'
                
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
                
                // Install dependencies if not already installed
                sh 'npm install'
                
                // Run tests (CI mode untuk non-interactive)
                // Set NODE_OPTIONS for compatibility
                sh 'CI=true NODE_OPTIONS=--openssl-legacy-provider npm test'
                
                echo 'Tests completed successfully'
            }
        }
        
        // Stage 4: Archive Artifacts
        stage('Archive') {
            echo '========================================='
            echo 'Stage: Archive Build Artifacts'
            echo '========================================='
            
            // Archive the build folder
            archiveArtifacts artifacts: 'build/**/*', fingerprint: true
            
            // Archive package.json for reference
            archiveArtifacts artifacts: 'package.json', fingerprint: true
            
            echo 'Artifacts archived successfully'
        }
        
        // Success notification
        echo '========================================='
        echo 'Pipeline completed successfully! ✓'
        echo '========================================='
        currentBuild.result = 'SUCCESS'
        
    } catch (Exception e) {
        // Error handling
        echo '========================================='
        echo 'Pipeline failed! ✗'
        echo "Error: ${e.getMessage()}"
        echo '========================================='
        currentBuild.result = 'FAILURE'
        throw e
        
    } finally {
        // Cleanup
        stage('Cleanup') {
            echo '========================================='
            echo 'Stage: Cleanup'
            echo '========================================='
            
            // Clean up workspace if needed
            echo 'Cleaning up workspace...'
            
            // Optional: Remove Docker containers
            sh """
                docker ps -a | grep ${containerName} | awk '{print \$1}' | xargs -r docker rm -f || true
            """
            
            echo 'Cleanup completed'
        }
        
        // Send notification about build status
        echo "Build Status: ${currentBuild.result}"
    }
}
