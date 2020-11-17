node{
    def mvn = tool 'maven'
    environment {
        // This can be nexus3 or nexus2 server
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "http"
        // Where your Nexus is running
        NEXUS_URL = "20.44.100.76:8081"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY_RELEASES = "mvn_nexus"
        NEXUS_REPOSITORY_SNAPSHOTS = "mvn_nexus"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "cfd20180-24c0-42da-97d6-9597f9f2ca3b"
    }
    stage('GIT SCM'){
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'LV_git', url: 'https://github.com/virdilokeshna/game-of-life.git']]])
    }
    stage('cleanInstall'){
        withMaven(maven: 'maven') {
            echo '***************INITIALISING*************'
            sh 'mvn -f pom.xml clean compile install'
        }
    }
    stage('run sonarscan'){
            withMaven(maven: 'maven'){
            
                echo '***************INITIALISING*************'
                sh 'mvn -f pom.xml sonar:sonar'
                }
    }
    stage('Nexus Repository') {
          //  steps {
                script {
                    pom = readMavenPom file: 'pom.xml';
                    filesByGlob = findFiles(glob: 'target/*.${pom.packaging}');
                    echo '${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}'
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo '*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}';
                        nexusArtifactUploader(
                                nexusVersion: NEXUS_VERSION,
                                protocol: NEXUS_PROTOCOL,
                                nexusUrl: NEXUS_URL,
                                groupId: pom.groupId,
                                version: pom.version,
                                repository: NEXUS_REPOSITORY_SNAPSHOTS,
                                credentialsId: NEXUS_CREDENTIAL_ID, 
                                artifacts: [
                                    [artifactId: pom.artifactId, 
                                     classifier: '',
                                     file: artifactPath,
                                     type: pom.packaging],
                                    [artifactId: pom.artifactId,
                                     classifier: '',
                                     file: 'pom.xml', 
                                     type: 'pom']]);
                      
                    } else {
                        error '*** File: ${artifactPath}, could not be found';
                    }
                }
           // }
        }
   
}