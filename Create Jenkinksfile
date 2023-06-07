pipeline {
    agent {
		label "JenkinsSlave"
	}
    stages {
        stage('Notification') {
            steps {
                emailext attachLog:true,
                body: "$JOB_NAME - Build # $BUILD_NUMBER - $currentBuild.currentResult :\nCheck console output at $BUILD_URL to view the results.", 
                compressLog: true, subject: "$JOB_NAME - Build # $BUILD_NUMBER - $currentBuild.currentResult !", to: 'trugly.csenge@gmail.com,csencsi5197@gmail.com'
            }
        }
        stage('Code Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'CsengeGitHub', url: 'https://github.com/CsengeDeak-Trugly/SonarQubeNodeJS.git']])
            }
        }
		stage('Code Build') {
            steps {
                echo 'Code Build'
				sh """
					ls -lart
					npm install
					npm run build
				"""
            }
        }
		stage('Test Automation') {
            steps {
                echo 'Test Automation'
				sh """
					npm run test
					npm run test:e2e
				"""
            }
        }
		stage('Code Deployment') {
            steps {
                echo 'Code Deployment'
            }
        }
    }
    post {
		success {
			echo "Build is Success !!!!"
		}
		
        failure {    
			echo "Build is Failure !!!!"
        }
		always {
			emailext attachLog: true, 
                body: "$JOB_NAME - Build # $BUILD_NUMBER - $currentBuild.currentResult :\nCheck console output at $BUILD_URL to view the results.", 
                compressLog: true, subject: "$JOB_NAME - Build # $BUILD_NUMBER - $currentBuild.currentResult !", to: 'csencsi5197@gmail.com'
		}
    }
}
