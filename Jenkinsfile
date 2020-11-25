pipeline{
	agent any
    tools{
        maven 'maven'
    }
	stages{
        stage('GIT SCM'){
            steps
			{
				checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'LV_git', url: 'https://github.com/virdilokeshna/game-of-life.git']]])
			}
		}
		stage('Build Project'){
            steps
			{
				echo '***************INITIALISING*************'
				sh 'mvn -f pom.xml clean compile install'
			}
		}
		stage('Sonar Scan'){
            steps
			{
				echo '***************INITIALISING*************'
                sh 'mvn -f pom.xml sonar:sonar'
			}
		}
		stage('Publish Artifact'){
            steps
			{
				echo 'push artifacts to repository'
				script 
                {
                    pom = readMavenPom file: 'pom.xml'
                    imodules = pom.modules
                    for (String i : imodules) {
                        pomModule = readMavenPom file: i + '/pom.xml'
                        
                        nexusArtifactUploader artifacts: [
                            [artifactId: pomModule.artifactId, 
                            classifier: '', 
                            file: pomModule.artifactId + '/pom.xml', 
                            type: pomModule.packaging]
                            ], 
                            credentialsId: 'Nexus_ID',
                            groupId: pom.groupId, 
                            nexusUrl: '104.209.135.134:8081/nexus', 
                            nexusVersion: 'nexus2', 
                            protocol: 'http', 
                            repository: 'GameofLife', 
                            version: pom.version
                    }   
                    nexusArtifactUploader artifacts: [
                       [artifactId: pom.artifactId, 
                       classifier: '', 
                       file: 'pom.xml', 
                       type: pom.packaging]
                       ], 
                       credentialsId: 'Nexus_ID',
                       groupId: pom.groupId, 
                       nexusUrl: '104.209.135.134:8081/nexus', 
                       nexusVersion: 'nexus2', 
                       protocol: 'http', 
                       repository: 'GameofLife', 
                       version: pom.version
			    }
            }
		}
		
	}
}