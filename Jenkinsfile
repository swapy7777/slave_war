pipeline{
    agent{
        label{
            label "slave1"
            customWorkspace "/mnt/test"
        }
    }
    tools {
        maven "maven"
    }
    stages {
        stage("cloning"){
            steps {
                sh "rm -rf *"
                sh "git clone https://github.com/snehaos20/project.git"
                echo "cloning"
                sh "chmod -R 777 /mnt/test/"
            }
        }
        stage("building war"){
            steps {
                sh "mvn -f /mnt/test/project clean install"
            }
        }
        stage("upload file"){
            steps {

                    withAWS(region:'ap-south-1',credentials:'s3_access') {
                   // def identity=awsIdentity();
                // Upload files from working directory to project workspace
                        echo "s3upload"
                    s3Upload(bucket:"test-s3buck123", workingDir:'/mnt/test/project/target', includePathPattern:'**/*.war');
                    }
            }
        }
        stage("deploying"){
            steps {
                sh "cp /mnt/test/project/target/*.war /mnt/server/apache-tomcat-9.0.83/webapps"
                sh "/mnt/apache-tomcat-9.0.83/bin/startup.sh"
                
            }
        }
    }
}
