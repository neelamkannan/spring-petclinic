pipeline {
    agent any
   
    stages {
        stage('vcs') {
            steps {
                git branch: "main", url: 'https://github.com/neelamkannan/spring-petclinic.git'
            }
            
        }
         stage ('Artifactory configuration') {
            steps {
                
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "jfrog_123",
                    releaseRepo: 'qt-libs-release-local',
                    snapshotRepo: 'qt-libs-snapshot-local'
                )

            }
        }
        stage('Build the Code') {
            steps {
                withSonarQubeEnv('sonar_admin') {
                    sh script: 'mvn clean package sonar:sonar'
                }
            }
        }
            

          stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'maven', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'package',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrog_123"
                )
            }
        }
        
    }
}
