pipeline {
    agent any

    environment {
        RESOURCE_GROUP = "jenkins-rg"
        LOCATION = "uksouth"
        STORAGE_ACCOUNT = "jenkinsstorage${BUILD_NUMBER}"
    }

    stages {

        stage('Login to Azure') {
            steps {
                withCredentials([
                    string(credentialsId: 'azure-client-id', variable: 'CLIENT_ID'),
                    string(credentialsId: 'azure-client-secret', variable: 'CLIENT_SECRET'),
                    string(credentialsId: 'azure-tenant-id', variable: 'TENANT_ID')
                ]) {
                    sh '''
                    az login --service-principal \
                      --username $CLIENT_ID \
                      --password $CLIENT_SECRET \
                      --tenant $TENANT_ID
                    '''
                }
            }
        }

        stage('Create Storage Account') {
            steps {
                sh '''
                az storage account create \
                  --name $STORAGE_ACCOUNT \
                  --resource-group $RESOURCE_GROUP \
                  --location $LOCATION \
                  --sku Standard_LRS
                '''
            }
        }
    }
}