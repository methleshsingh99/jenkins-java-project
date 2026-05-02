pipeline {
    agent any

    environment {
        BRANCH = 'main'
        S3_BUCKET = 'methlesh-cicd-bucket'
        REGION = 'ap-south-2'
        TOMCAT_PATH = '/opt/tomcat'
    }

    tools {
        maven 'maven'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: "${BRANCH}", url: 'https://github.com/methleshsingh99/jenkins-cicd-tomcat-deployment.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package Artifact') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Upload to S3') {
            steps {
                s3Upload(
                    consoleLogLevel: 'INFO',
                    dontSetBuildResultOnFailure: false,
                    dontWaitForConcurrentBuildCompletion: false,
                    pluginFailureResultConstraint: 'FAILURE',
                    profileName: 'methlesh-cicd',
                    userMetadata: [],
                    entries: [[
                        bucket: "${S3_BUCKET}",
                        sourceFile: 'target/*.war',
                        selectedRegion: "${REGION}",
                        flatten: false,
                        gzipFiles: false,
                        keepForever: false,
                        managedArtifacts: false,
                        noUploadOnFailure: false,
                        showDirectlyInBrowser: false,
                        storageClass: 'STANDARD',
                        uploadFromSlave: false,
                        useServerSideEncryption: false
                    ]]
                )
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh """
                    echo "Getting WAR file..."
                    WAR_FILE=\$(ls target/*.war | head -n 1)
                    FILE_NAME=\$(basename \$WAR_FILE)

                    echo "Downloading WAR from S3..."
                    aws s3 cp s3://${S3_BUCKET}/\$FILE_NAME /tmp/\$FILE_NAME

                    echo "Deploying WAR to Tomcat..."
                    mv /tmp/\$FILE_NAME ${TOMCAT_PATH}/webapps/ROOT.war

                    echo "Restarting Tomcat..."
                    sudo systemctl restart tomcat

                    echo "Waiting for app to start..."
                    sleep 10

                    echo "Deployment Successful!"
                """
            }
        }

    }

    post {
        success {
            echo '🎉 CI/CD Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs.'
        }
    }
}
