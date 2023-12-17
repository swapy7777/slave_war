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
        stage("upload in s3"){
            steps{
                
        def identity=awsIdentity();
        s3Upload acl: 'PublicRead', bucket: 's3-snehabuck', file: '/mnt/test/project/target/*.war', path: "war", workingDir: '.'
            }
        }
        stage("deploying"){
            steps {
                sh "cp /mnt/test/project/target/*.war /mnt/apache-tomcat-9.0.83/webapps"
                sh "/mnt/apache-tomcat-9.0.83/bin/startup.sh"
                
            }
        }
    }
}
