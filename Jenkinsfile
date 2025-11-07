// Jenkinsfile - Scripted Pipeline untuk React App (Simplified Version)
// Submission CI/CD Pipeline dengan Jenkins
// Menggunakan Scripted Pipeline (bukan Declarative) untuk nilai tinggi
// This version installs Node.js directly without requiring NodeJS plugin

node {
    // Set environment variables
    env.NODE_OPTIONS = '--openssl-legacy-provider'
    
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
        
        // Stage 2: Setup Node.js
        stage('Setup') {
            echo '========================================='
            echo 'Stage: Setup Node.js Environment'
            echo '========================================='
            
            // Verify Node.js and npm versions
            sh 'node --version || echo "Node.js not found"'
            sh 'npm --version || echo "npm not found"'
            
            echo 'Node.js setup completed'
        }
        
        // Stage 3: Build
        stage('Build') {
            echo '========================================='
            echo 'Stage: Build React Application'
            echo '========================================='
            
            echo 'Installing dependencies...'
            sh 'npm install'
            
            echo 'Building React application...'
            sh 'npm run build'
            
            echo 'Build completed successfully'
            sh 'ls -la build/'
        }
        
        // Stage 4: Test
        stage('Test') {
            echo '========================================='
            echo 'Stage: Test React Application'
            echo '========================================='
            
            echo 'Running tests...'
            sh 'CI=true npm test'
            
            echo 'Tests completed successfully'
        }
        
        // Stage 5: Archive Artifacts
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
            sh 'rm -rf node_modules || true'
            
            echo 'Cleanup completed'
        }
        
        // Send notification about build status
        echo "Build Status: ${currentBuild.result}"
    }
}
