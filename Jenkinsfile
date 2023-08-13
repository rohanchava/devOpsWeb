// pipeline {
//     agent any
    
//     // tools {
//     //     maven 'local_maven'
//     // }
//     // parameters {
//     //      string(name: 'staging_server', defaultValue: '13.232.37.20', description: 'Remote Staging Server')
//     // }

// stages{
//         stage('Build'){
//             steps {
//                 sh 'mvn clean package'
//             }
//             post {
//                 success {
//                     echo 'Archiving the artifacts'
//                     archiveArtifacts artifacts: '**/target/*.war'
//                 }
//             }
//         }

//         stage ('Deployments'){
//             parallel{
//                 stage ("Deploy to Staging"){
//                     steps {
//                         sh "scp -v -o StrictHostKeyChecking=no **/*.war root@${params.staging_server}:/opt/tomcat/webapps/"
//                     }
//                 }
//             }
//         }
//     }
// }

pipeline {
    agent any
    
    environment {
        TOMCAT_URL = "http://35.173.127.199:9090/manager/text"
        TOMCAT_USERNAME = "admin"
        TOMCAT_PASSWORD = "admin"
        WAR_FILE = "target/sample.war"
        SSH_KEY_PATH = "/Users/ravitejabojja/Documents/keys/ravi01bgmailtomcatserverEC2.pem"
        EC2_USER = "ec2-user"
        EC2_HOST = "35.173.127.199"
        //EC2
        // TOMCAT_URL = "http://localhost:9090/manager/text"
        // TOMCAT_USERNAME = "admin"
        // TOMCAT_PASSWORD = "admin"
        // WAR_FILE = "target/sample.war"
        // SSH_KEY_PATH = "/Users/ravitejabojja/Documents/keys/ravi01bgmailtomcatserverEC2.pem"
        // EC2_USER = "ec2-user"
        // EC2_HOST = "localhost"
    }

    stages {
        // stage('Checkout') {
        //     steps {
        //         // Checkout your source code from the repository
        //         // Use the appropriate SCM plugin or Git command
        //         // Example: git url: 'https://github.com/your/repo.git', branch: 'main'
        //     }
        // }
        
        stage('Build') {
            steps {
                // Build the WAR file using Maven
                sh "mvn clean package"
            }
        }
        
        // stage('Deploy to Tomcat') {
        //     steps {
        //         // Deploy the WAR file to Tomcat using manager-script
        //         sh """
        //         curl -v -u ${TOMCAT_USERNAME}:${TOMCAT_PASSWORD} \
        //         -T ${WAR_FILE} \
        //         "${TOMCAT_URL}/deploy?path=/sample&update=true"
        //         """
        //     }
        // }
        
        stage('Deploy to EC2') {
            steps {
                // Copy the WAR file to the EC2 instance
                sh """
                scp -i ${SSH_KEY_PATH} ${WAR_FILE} ${EC2_USER}@${EC2_HOST}:/home/ec2-user/apache-tomcat-8.5.91/webapps/
                cd ../bin; ./shutdown.sh
                cd ./startup.sh
                cd ../logs; cat catalina.out
                """
            }
        }
    }
}
















