node{
    def mvn = tool 'maven'
    
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
    stage('publish artifact'){
             
            echo 'push artifacts to repository'
            script {
                   pom = readMavenPom file: 'pom.xml'
                  // echo pom.artifactId
                   // filesByGlob = findFiles(glob: 'target/*.${pom.packaging}')
                   // echo '${filesByGlob[0].name}'
                  //  artifactPath = filesByGlob[0].path
                  //  echo artifactPath

                   nexusArtifactUploader artifacts: [
                       [artifactId: pom.artifactId, 
                       classifier: '', 
                       file: 'pom.xml', 
                       type: pom.packaging]
                       /*,
                       [artifactId: pom.artifactId, 
                       classifier: '', 
                       file: '~/' & pom.artifactId & '-build/pom.xml', 
                       type: 'jar']*/
                       ], 
                       credentialsId: 'cfd20180-24c0-42da-97d6-9597f9f2ca3b', 
                       groupId: pom.groupId, 
                       nexusUrl: '20.44.100.76:8081/nexus', 
                       nexusVersion: 'nexus2', 
                       protocol: 'http', 
                       repository: 'GameofLife', 
                       version: pom.version
                }   

   }
   
}