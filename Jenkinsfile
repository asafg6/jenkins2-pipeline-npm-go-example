
pipeline {
    agent none
	stages {
		stage('Build') {
            parallel {
                stage('Build Go') {
                    agent { label "go" }
                    steps {
                        sh 'bash build_go.bash'
                        stash includes: '**/go-app/main', name: 'go-app'
                    }
                }
                stage('Build Angular') {
                    agent { label "npm" }
                    steps {
                        sh 'bash build_angular.bash'
                        stash includes: '**/angular-app/dist/**', name: 'angular-app'
                    }
                }
            }

		}
		stage('Package') {
            agent { label 'master' }
			steps {
                dir("$WORKSPACE/my_app"){
                    unstash 'go-app'	
                    unstash 'angular-app'	
                }
                sh 'tar -czvf app.${BUILD_ID}.tar.gz my_app'
                archiveArtifacts "**/*.tar.gz"
            }
		}
	}
}