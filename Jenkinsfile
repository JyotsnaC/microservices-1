pipeline {
    agent any
    tools {
        jdk 'jdk8'
    }
    environment{
        server = null
        rtMaven = null
        buildInfo = null
    }
    stages {
        stage ('Initialize') {
            steps {
                script{
                    //server = Artifactory.newServer url: 'http://localhost:8081/artifactory', username: 'admin', password: 'admin'
                    rtMaven = Artifactory.newMavenBuild()
                    rtMaven.resolver releaseRepo: 'all-repo', snapshotRepo: 'all-repo'
                    rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local'
                    //rtMaven.deployer.deployArtifacts = false
                    rtMaven.tool = 'maven-3.5.4'
                    sh '''
                        echo "PATH = ${PATH}"
                        echo "M2_HOME = ${M2_HOME}"
                    '''
                    
                }
            }
            
        }

        stage ('Build') {
            steps {
                script{
                    
                    buildInfo = rtMaven.run pom: 'pom.xml', goals: 'clean install'
                }
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml' 
                }
            }
        }
       // stage('Publish'){
       //     steps{
       //         script{
       //             
       //             rtMaven.deployer.deployArtifacts buildInfo
       //             server.publishBuildInfo buildInfo
       //
       //         }
       //     }
       // }
    }
}
