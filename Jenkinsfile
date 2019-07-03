pipeline {
    agent {
	any{
        	image 'maven:3-alpine'
        	args '-v /root/.m2:/root/.m2'
	}
    }
    stages {
        stage('Build') {
            steps {
		sh 'cd main; mvn -e -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
		sh 'cd main; mvn test'
            }
            post {
                always {
                    junit 'main/commons/target/surefire-reports/*.xml'
                    junit 'main/keychain/target/surefire-reports/*.xml'
                    junit 'main/launcher/target/surefire-reports/*.xml'
                    junit 'main/ui/target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
