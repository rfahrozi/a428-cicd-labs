// Jenkinsfile - Scripted Pipeline untuk React App (Without Docker)
// Submission CI/CD Pipeline dengan Jenkins

node {
    try {
        stage('Checkout') {
            echo '========================================='
            echo 'Stage: Checkout Source Code'
            echo '========================================='
            checkout scm
            echo 'Source code checked out successfully'
            sh 'ls -la'
        }
        
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
        
        stage('Test') {
            echo '========================================='
            echo 'Stage: Test React Application'
            echo '========================================='
            echo 'Running tests...'
            sh 'CI=true npm test'
            echo 'Tests completed successfully'
        }
        
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
            sh 'rm -rf node_modules || true'
            echo 'Cleanup completed'
        }
        echo "Build Status: ${currentBuild.result}"
    }
}
