// pipeline {
// 	agent none

// 	triggers {
// 		pollSCM 'H/10 * * * *'
// 	}

// 	options {
// 		disableConcurrentBuilds()
// 		buildDiscarder(logRotator(numToKeepStr: '14'))
// 	}

// 	stages {
// 		stage("test: baseline (jdk8)") {
// 			agent {
// 				docker {
// 					image 'adoptopenjdk/openjdk8:latest'
// 					args '-v $HOME/.m2:/tmp/jenkins-home/.m2'
// 				}
// 			}
// 			options { timeout(time: 30, unit: 'MINUTES') }
// 			steps {
// 				sh 'test/run.sh'
// 			}
// 		}

// 	}

// 	post {
// 		changed {
// 			script {
// 				slackSend(
// 						color: (currentBuild.currentResult == 'SUCCESS') ? 'good' : 'danger',
// 						channel: '#sagan-content',
// 						message: "${currentBuild.fullDisplayName} - `${currentBuild.currentResult}`\n${env.BUILD_URL}")
// 				emailext(
// 						subject: "[${currentBuild.fullDisplayName}] ${currentBuild.currentResult}",
// 						mimeType: 'text/html',
// 						recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'RequesterRecipientProvider']],
// 						body: "<a href=\"${env.BUILD_URL}\">${currentBuild.fullDisplayName} is reported as ${currentBuild.currentResult}</a>")
// 			}
// 		}
// 	}
// }


pipeline {
    agent aws

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sahu04/hello-world.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn -f complete/pom.xml clean install'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'complete/target/*.jar', fingerprint: true
            }
        }

        stage('Run JAR') {
            steps {
                sh 'java -jar complete/target/gs-maven-0.1.0.jar'
            }
        }
    }

    post {
        always {
            echo "Build Finished"
        }
    }
}

