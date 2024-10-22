pipeline {
    agent any
    
    options {
        ansiColor('gnome-terminal')
        disableConcurrentBuilds()
        buildDiscarder(logRotator(numToKeepStr: '20', artifactNumToKeepStr: '20'))
    }
    environment {        
        SERVICE = "blockscout-backend" // repo name = ArgoCD service name (-argupd)
        GIT_REPO_URL = 'git@github.com:Gunzilla-Games/blockscout-backend.git'  // Set your repo URL here
        DOCKER_REGISTRY = "registry.digitalocean.com/gunz-images"
        DOCKER_REGISTRY_CREDENTIAL = "do-registry"
        DOCKER_BUILDKIT="1"  
        IS_GENERIC_TRIGGER = false 
        KUBECTL_CONFIG_LIVE = credentials("kub-otg_live")
        KUBECTL_CONFIG_STAGE = credentials("kub-otg_stage")
        GIT_TAG = "${tag}"
        SHORT_COMMIT_HASH = ''   
        GITHUB_EVENT_ACTION = "${action}" 
        GITHUB_PR_MERGED = "${merged}"
        GITHUB_REPO_NAME = "${reponame}"
        GITHUB_REPO_NAME_DEV = "${reponamedev}"
        // GENERIC_TRIGGER_TOKEN = credentials('GenericTriggerToken')  // Load the token from Jenkins credentials
    }

    triggers {
        GenericTrigger(
            genericVariables: [
                [key: 'ref', value: '$.ref'],
                [key: 'tag', value: '$.release.tag_name'],
                [key: 'action', value: '$.action'],
                [key: 'merged', value: '$.pull_request.merged'],
                [key: 'reponame', value: '$.repository.name'],
                [key: 'reponamedev', value: '$.pull_request.base.repo.name']
            ],
            causeString: 'Triggered on $tag',
            token: '11cf45ec680e248eb76c4942e847549a95blockscout-backend',  // optional
            printContributedVariables: true,
            printPostContent: true,
            silentResponse: false
        )    
    }

    stages {
        // stage('Checkout') {
        //     when {
        //         expression { return  GIT_TAG ==~ /^v\d+\.\d+\.\d+(-rc\.\d+)?$/  } 
        //     }
        //     steps {
        //         script {
        //             withCredentials([usernamePassword(credentialsId: 'c5302be4-075a-4500-b1fd-ed3148d0d24f', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {

        //                 // Perform the checkout
        //                 checkout([$class: 'GitSCM',
        //                     branches: [[name: "refs/tags/${tag}"]],
        //                     userRemoteConfigs: [[url: "https://${GIT_USERNAME}:${GIT_PASSWORD}@${GIT_REPO_URL}"]]
        //                 ])
        //             }
        //         }
        //     }
        // }
        // stage('Set tag releases') {
        //     when {
        //         expression { return  GITHUB_EVENT_ACTION == 'published'  } // Run this stage only if triggered by GenericTrigger
        //     }
        //     steps {              
        //         echo "On stage: ${STAGE_NAME}"
        //         sh "env"
        //         script {
        //             // Get the current Git tag
        //             // GIT_TAG = sh(script: "git describe --tags `git rev-list --tags --max-count=1`", returnStdout: true).trim()
        //             // GIT_TAG = tag
        //             echo "Checked out code. Current Git tag: ${GIT_TAG}" 

        //             // Fetch the latest tags and branches with authentication
        //             withCredentials([usernamePassword(credentialsId: 'c5302be4-075a-4500-b1fd-ed3148d0d24f', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]) {
                        
        //                 sh 'git fetch --tags --force --quiet https://${GIT_USERNAME}:${GIT_PASSWORD}@${GIT_REPO_URL}'
        //             }

        //             // Define the pattern for the desired tags (e.g.,  v1.0.0 or v1.0.0-rc0)
        //             def tagPattern = /^v\d+\.\d+\.\d+(-rc\.\d+)?$/
                    
        //             // Check if the tag name matches the pattern
        //             if (!GIT_TAG.matches(tagPattern)) {
        //                 error "Tag ${GIT_TAG} does not match the pattern ${tagPattern}"
        //             }
                    
        //         }
        //     }
        // }
 
        // stage('Docker build releases') {
        //     when {
        //         expression { return GITHUB_EVENT_ACTION == 'published' && GITHUB_REPO_NAME == SERVICE } // Run this stage only if triggered by GenericTrigger
        //     }
        //     steps {
        //         script {
                    
        //             echo "On stage: ${STAGE_NAME}"
        //             sh "eval `ssh-agent -s`"  
        //             sh "env"                        
        //              if (GIT_TAG ==~ /^v\d+\.\d+\.\d+$/) {
        //                 // Build and tag the Docker image with both the GIT_TAG and latest
        //                     sh "docker build --ssh default=/var/lib/jenkins/.ssh/id_ecdsa -t ${DOCKER_REGISTRY}/${SERVICE}:${GIT_TAG} -t ${DOCKER_REGISTRY}/${SERVICE}:latest ."
        //              } else {
        //                 // Build and tag the Docker image with the GIT_TAG only
        //                sh "docker build --ssh default=/var/lib/jenkins/.ssh/id_ecdsa -t ${DOCKER_REGISTRY}/${SERVICE}:${GIT_TAG} ."
        //              }
        //         }
        //     }
        // }
        
        // stage('Docker Push releases') {
        //     when {
        //         expression { return  GITHUB_EVENT_ACTION == 'published' && GITHUB_REPO_NAME == SERVICE } // Run this stage only if triggered by GenericTrigger
        //     }
        //     steps {
        //         script {
        //             echo "Pushing Docker image for tag: ${GIT_TAG}"
        //             // Push the Docker image with the GIT_TAG
        //             docker.withRegistry("https://${DOCKER_REGISTRY}", DOCKER_REGISTRY_CREDENTIAL) {
        //                      sh "docker push ${DOCKER_REGISTRY}/${SERVICE}:${GIT_TAG}"}
        //             // Optionally, push the latest tag if it was built
        //             if (GIT_TAG ==~ /^v\d+\.\d+\.\d+$/) {
        //                 docker.withRegistry("https://${DOCKER_REGISTRY}", DOCKER_REGISTRY_CREDENTIAL) {
        //                      sh "docker push ${DOCKER_REGISTRY}/${SERVICE}:latest"}
        //             }
        //         }
        //     }
        // }

        // stage('ArgoCd app sync otg_stage') {
        //     when {
        //         expression { return GITHUB_EVENT_ACTION == 'published' && GIT_TAG ==~ /^v\d+\.\d+\.\d+(-rc\.\d+)?$/ && GITHUB_REPO_NAME == SERVICE }
        //     }
        //     environment {
        //         ARGOCD_SERVER = 'otg-stage-argo.gunztoken.cc'
        //         ARGOCD_USERNAME = 'admin'
        //         ARGOCD_PASSWORD = credentials('otg-stage-argo')
        //     }
        //     steps {
        //          sh "sleep 130"
        //          sh '''
        //             argocd login $ARGOCD_SERVER --username $ARGOCD_USERNAME --password $ARGOCD_PASSWORD --insecure
        //             '''
        //          sh "argocd app sync ${SERVICE}-argupd"                             
        //     }
            //  post {
                
            //     success {
            //         build job: '../otg-api-tests/otg-stage', propagate: true, wait: true
            //     }
                
            // }
        // }

        // stage('ArgoCd app sync otg_live') {
        //     when {
        //         expression { return GITHUB_EVENT_ACTION == 'published' && GIT_TAG ==~ /^v\d+\.\d+\.\d+?$/ && GITHUB_REPO_NAME == SERVICE }
        //     } 
        //     environment {
        //         ARGOCD_SERVER = 'otg-live-argo.gunztoken.cc'
        //         ARGOCD_USERNAME = 'admin'
        //         ARGOCD_PASSWORD = credentials('otg-live-argo')
        //     }
        //     steps {
        //          sh "sleep 130"
        //          sh '''
        //             argocd login $ARGOCD_SERVER --username $ARGOCD_USERNAME --password $ARGOCD_PASSWORD --insecure
        //             '''
        //          sh "argocd app sync ${SERVICE}-argupd"                              
        //     }
        // }

        stage('Docker build otg_dev') {
            // when {
            //     expression { return  GITHUB_PR_MERGED == 'true' && GITHUB_REPO_NAME_DEV == SERVICE } // Run this stage only if triggered by githubPush
            // }
            steps {
                script {
                    // Get the shortened commit hash
                    SHORT_COMMIT_HASH = sh(
                        script: 'git rev-parse --short HEAD',
                        returnStdout: true
                    ).trim()
                    echo "On stage: ${STAGE_NAME}"
                      
                    sh "env"                        
                    sh "docker build  -t ${DOCKER_REGISTRY}/${SERVICE}:otg_dev -t ${DOCKER_REGISTRY}/${SERVICE}:${SHORT_COMMIT_HASH} -f docker/Dockerfile ."
                }
            }
        }

        stage('Docker Push otg_dev') {
            // when {
            //     expression { return GITHUB_PR_MERGED == 'true' && GITHUB_REPO_NAME_DEV == SERVICE } // Run this stage only if triggered by githubPush
            // }
            steps {
                script {
                    echo "Pushing Docker image for tag: otg_dev"
                    docker.withRegistry("https://${DOCKER_REGISTRY}", DOCKER_REGISTRY_CREDENTIAL) {
                             sh "docker push ${DOCKER_REGISTRY}/${SERVICE}:${SHORT_COMMIT_HASH}"
                             sh "docker push ${DOCKER_REGISTRY}/${SERVICE}:otg_dev"
                    }
                }
            }
        }
        
        // stage('ArgoCd app sync otg_dev') {
        //     when {
        //          expression { return GITHUB_PR_MERGED == 'true' && GITHUB_REPO_NAME_DEV == SERVICE } // Run this stage only if triggered by githubPush
        //      } 
        //     environment {
        //         ARGOCD_SERVER = 'otg-dev-argo.gunztoken.cc'
        //         ARGOCD_USERNAME = 'admin'
        //         ARGOCD_PASSWORD = credentials('otg-dev-argo')
        //     }
        //     steps {
        //          sh "sleep 130"
        //          sh '''
        //             argocd login $ARGOCD_SERVER --username $ARGOCD_USERNAME --password $ARGOCD_PASSWORD --insecure
        //             '''
        //          sh "argocd app sync ${SERVICE}-argupd"                              
        //     }
        
            // post {
                
            //     success {
            //         build job: '../otg-api-tests/otg-dev', propagate: true, wait: true
            //     }
        // }
    }
}
