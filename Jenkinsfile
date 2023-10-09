pipeline {
        agent any
        tools {
        maven "Maven3.6.3"
        jdk "JDK"
        }

//        environment {
//        // This can be nexus3 or nexus2
//        NEXUS_VERSION = "nexus3"
//        // This can be http or https
//        NEXUS_PROTOCOL = "http"
//        // Where your Nexus is running
//        NEXUS_URL = "10.97.72.168:8081"
//        // Repository where we will upload the artifact
//        NEXUS_REPOSITORY = "LoginWebApp"
//        // Jenkins credential id to authenticate to Nexus OSS
//        NEXUS_CREDENTIAL_ID = "nexusCredential"
//        ARTIFACT_VERSION = "${BUILD_NUMBER}"
//        }

        stages {

        stage("Check out") {
        steps {
        script {
        git branch: 'feature/nexusUpload', url: 'https://github.com/KaylerLee/nexus-test.git';
        }
        }
        }

        stage("mvn build") {
        steps {
        script {
        sh "mvn clean package"
        }
            post{
                success{
                    echo "Archiving the artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        }
        stage ("Deploy to tomcat server") {
            step{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-admin', path: '', url: '10.97.72.168:8888')], contextPath: null, war: '**/*.war'
            }
        }

//        stage("publish to nexus") {
//        steps {
//        script {
//        // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps
//        pom = readMavenPom file: "pom.xml";
//        // Find built artifact under target folder
//        filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
//        // Print some info from the artifact found
//        echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
//        // Extract the path from the File found
//        artifactPath = filesByGlob[0].path;
//        // Assign to a boolean response verifying If the artifact name exists
//        artifactExists = fileExists artifactPath;
//
//        if(artifactExists) {
//        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
//
//        nexusArtifactUploader(
//        nexusVersion: NEXUS_VERSION,
//        protocol: NEXUS_PROTOCOL,
//        nexusUrl: NEXUS_URL,
//        groupId: pom.groupId,
//        version: ARTIFACT_VERSION,
//        repository: NEXUS_REPOSITORY,
//        credentialsId: NEXUS_CREDENTIAL_ID,
//        artifacts: [
//        // Artifact generated such as .jar, .ear and .war files.
//        [artifactId: pom.artifactId,
//        classifier: '',
//        file: artifactPath,
//        type: pom.packaging]
//        ]
//        );

//        }
//        else {
//        error "*** File: ${artifactPath}, could not be found";
        }
        }
//       }
//        }

//        }
//        }
