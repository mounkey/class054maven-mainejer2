pipeline {
    agent any
        stages {

        stage('Build') {
            steps {
                sh 'mvn -DskipTests clean package'

            }
        }

         stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo " File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: "nexus3",
                            protocol: "http",
                            nexusUrl: "localhost:8081",
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: "Repositorio3",
                            credentialsId: "NexusConexion",
                            artifacts: [
                                [artifactId: pom.artifactId,
                                        classifier: '',
                                        file: artifactPath,
                                        type: pom.packaging]
                            ]
                        );
                    } else {
                        error "* File: ${artifactPath}, could not be found";
                    }
                }
            }

            } 
     }
        post {
      success {
        slackSend channel: '#fundamentos-devops', color: '#000', message: 'Funcionó :smile:  :star: ', teamDomain: 'sustantiva-sede', tokenCredentialId: 'Token-slack2', username: 'Juan Pablo Grover Pinto'
      }
   } 
}
