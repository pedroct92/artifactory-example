pipeline {
   agent any

   stages {
//        stage ('Clone') {
//            steps {
//                git branch: 'master', url: "https://github.com/pedroct92/artifactory-example.git"
//            }
//        }
       stage ('Config Build Info') {
           steps {
               rtBuildInfo (
                       captureEnv: true,
                       includeEnvPatterns: ["*"]
               )
           }
       }
       stage ('Build') {
           steps {
               rtGradleRun (
                   usesPlugin: true, // Artifactory plugin already defined in build script
                   tool: "g6", // Tool name from Jenkins configuration
                   buildFile: 'build.gradle',
                   tasks: 'clean build',
               )
           }
       }
       stage ('Publish') {
          steps {
              rtGradleRun (
                  usesPlugin: true, // Artifactory plugin already defined in build script
                  tool: "g6", // Tool name from Jenkins configuration
                  buildFile: 'build.gradle',
                  tasks: 'artifactoryPublish',
              )
          }
       }
       stage ('Publish build info') {
           steps {
               rtPublishBuildInfo (
                       serverId: "ART"
               )
           }
       }
   }
}
