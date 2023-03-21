pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'your.docker.registry.com'
        DOCKER_IMAGE_NAME = 'your-docker-image-name'
        DOCKER_IMAGE_TAG = 'latest'
    }


     stages {
    stage('Checkout') {
            steps {
             script {
                cleanWs()
                checkout([$class: 'GitSCM'
                    , branches: [[name: "*/master"]]
                    , doGenerateSubmoduleConfigurations: false
                    , extensions: [[$class: 'CheckoutOption', timeout: 120], [$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 180]]
                    , submoduleCfg: [], 
                    userRemoteConfigs: [[url: 'https://github.com/laribihoussem/vue-js-devops']]
                ])
             }
            }
        }

    stage('Build') {
        steps {
            sh 'npm install'
            sh 'npm run build'
            archiveArtifacts artifacts: 'dist/**', fingerprint: true
        }
    }


    stage('Build Docker Image') {
        steps {
            // Build a Docker image for your Vue.js application
            sh 'docker build -t vue-image .'
        }
        // Optionally, you can push the image to a Docker registry
    }

    // stage('Docker build and push') {
    //     steps {
    //         script {
    //             docker.build("${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}", "-f Dockerfile .")
    //             docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-creds') {
    //                 docker.push("${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
    //             }
    //         }
    //     }
    // }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }

        failure {
            echo 'Build and deployment failed!'
        }
    }
}


