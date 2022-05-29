pipeline{
    environment {
        imagename = "abeerab/backendimage"
        registryCredential = "dockerhub_credentials"
        scannerHome = tool name: 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
        // scannerHome = tool 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }
    agent any
    tools {
        nodejs "node-js"
    }
    stages{
        stage("test-sonar"){
            steps{
                script {
                    withSonarQubeEnv("sonarQube") {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=solarenergy-backend \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://54.217.252.229:9000/ \
                        -Dsonar.login=admin \
                        -Dsonar.password=user"
                    } 
                }
            }
        }
        stage("install dependencies"){
            
            steps{
                sh 'npm install'
            }
        }
        
        stage("docker-build"){
            steps{
                script {
                    dockerImage = docker.build imagename   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("deployment"){
            steps {
                echo "deployment is done"
                // withCredentials([
                // string(credentialsId: 'k8s', variable: 'api_token')
                // ]) {
                // sh 'kubectl --token $api_token --server http://192.168.49.2:42477 --insecure-skip-tls-verify=true apply -f ./kubernetes '
                // }
            }
        }
    }
}
