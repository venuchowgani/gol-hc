pipeline {
    agent { label 'JDK_17' }
    tools { 
        jdk 'JDK_8'
        maven 'DEFAULT_MAVEN'
    }
    stages {
        stage('VCS') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/venuchowgani/gol-hc.git'
            }
        }
        stage('JFROG') {
            steps{
                rtMavenDeployer (
                    id: 'maven-deployer',
                    serverId: JFROG_ID,
                    releaseRepo: 'maven_project-libs-release',
                    snapshotRepo: 'maven_project-libs-snapshot'
                )
                rtMavenRun (
                    tool: 'Maven 3.6.3',
                    pom: 'pom.xml',
                    goals: '-U clean install',
                    deployerId: "maven-deployer"
                )
                rtPublishBuildInfo (
                    serverId: "JFROG_ID"
                )
            }
        }
        
    }
}