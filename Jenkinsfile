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
				sh 'cd main; mvn -B -DskipTests clean package'
            }
        }
		stage('Quality Test') {
            steps {
                sh 'mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs com.github.spotbugs:spotbugs-maven-plugin:3.1.7:spotbugs'
            }
			post {
				always {
				    recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
				    recordIssues enabledForFailure: true, tool: checkStyle()
				    recordIssues enabledForFailure: true, tool: spotBugs()
				    recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
				    recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
				}
	    	}
		}
        stage('Unit Test') {
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
