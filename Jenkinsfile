pipeline{
    agent any
    	environment {
		notifyEmail ="divya.gilhotra01@nagarro.com"
	}
    tools{
        maven 'Maven'
    }
     triggers{
        cron 'H H 1,15 1-11 *'
   	}
    stages{
        stage("code checkout"){
            steps{
            checkout scm
            }
        }   
        stage("code build"){
            steps{
            bat "mvn clean install"
            }
        }
        stage("unit test"){
            steps{
            bat "mvn test"
            }
        }
        stage("Sonar Analysis"){
            steps{
             withSonarQubeEnv("sonarqube_test")
                 {
		bat "mvn --version"
                bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar"	 
                 //bat 'mvn sonar:sonar'
                 }
             }
        }
        stage("Publish to Artifactory"){
            steps{
                rtMavenDeployer(
                    id: 'deployer',
                    serverId: 'test-jfrog',
                    releaseRepo: 'devtestassignment',
                    snapshotRepo: 'devtestassignment'
                )
                rtMavenRun(
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer'
                    )
                rtPublishBuildInfo(
                    serverId:'test-jfrog',
                )
            }        
        }
        
    }
    post{
        success{
            bat "echo success"
            }
        }
}
