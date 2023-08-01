pipeline {

    agent { label 'JDK_17' }

    triggers { pollSCM('* * * * *') }

    tools {

        jdk 'JDK_17'

        # maven 'MAVEN-3.9'

    }

    stages {

        stage('VCS') {

            steps {

                git url: 'https://github.com/venuchowgani/gol-hc.git',

                    branch: 'master'

            }

        }      

        stage('version') {

            steps {

                sh 'java -version'

                sh 'mvn --version'

            }

        }

        stage('Publish artifacts to Jfrog') {

            steps {

                rtMavenDeployer (

                    tool: 'default'

                    id: "MAVEN_DEPLOYER",

                    serverId: "JFROG_ID",

                    releaseRepo: 'maven_project-libs-release',

                    snapshotRepo: 'maven_project-libs-snapshot'

                )

                rtMavenRun (                    

                    pom: 'pom.xml',

                    goals: 'clean install',

                    deployerId: "MAVEN_DEPLOYER"

                )

                rtPublishBuildInfo (

                    serverId: "Jfrog_Devops"

                )

            }

        }

        stage('Archiving the artifacts & publishing test results') {

            steps {

                archiveArtifacts onlyIfSuccessful: true,

                            artifacts: '**/target/*.war'

                junit testResults: '**/surefire-reports/TEST-*.xml'

            }

        }

    }

}