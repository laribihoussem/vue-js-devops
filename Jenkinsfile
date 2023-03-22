pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = '19982611'
        DOCKER_IMAGE_NAME = 'vue3app'
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
            bat 'npm install'
            bat 'npm run build'
            archiveArtifacts artifacts: 'dist/**', fingerprint: true
        }
    }


    stage('Build Docker Image and push') {
        steps {
            // Build a Docker image for your Vue.js application
            bat 'docker build -t vue-image .'
            bat'docker tag vue-image:latest 19982611/vue-image:latest'
            bat 'push 19982611/vue-image:latest'
            //bat 'docker push("${DOCKER_REGISTRY}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"'
// docker tag local-image:tagname new-repo:tagname
// docker push new-repo:tagname
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


