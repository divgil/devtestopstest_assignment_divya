pipeline{
    agent any
    	environment {
		notifyEmail ="divya.gilhotra01@nagarro.com"
	}
    //tools{
        //maven 'automaven'
   // }
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
             withSonarQubeEnv("test_sonar")
                 {
                 bat 'mvn sonar:sonar'
                 }
             }
        }
        stage("Publish to Artifactory"){
            steps{
                rtMavenDeployer(
                    id: 'deployer',
                    serverId: '123456789@artifactory',
                    releaseRepo: 'test-divya-jenkins',
                    snapshotRepo: 'test-divya-jenkins'
                )
                rtMavenRun(
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer'
                    )
                rtPublishBuildInfo(
                    serverId:'123456789@artifactory',
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
