pipeline {
    agent any
    environment {
        SONAR_HOME = tool "Sonar"
    }
    parameters {
        choice(
            name: 'DEPLOY_ENV',
            choices: ['dev', 'prod'],
            description: 'Choose the environment to deploy: DEV or PROD'
        )
    }

    stages {

        stage("Code Checkout") {
            steps {
                git(
                    url: "https://github.com/bhushanpatil251/devsecops-dify-cicd-pipeline",
                    branch: "main",
                    credentialsId: "jenkins-git",
                    changelog: true,
                    poll: true
                )
            }
        }

        stage('Verify Docker Hub Tags') {
            steps {
                script {
                    def images = [
                        "amigosnishant/dify-web-service",
                        "amigosnishant/dify-api-service"
                    ]
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'dockerhub-cred-id',
                            usernameVariable: 'DOCKER_USER',
                            passwordVariable: 'DOCKER_PASS'
                        )
                    ]) {
                        echo "Logging in to Docker Hub..."
                        sh '''
                            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        '''
                        echo "Deploying version v0.0.12 pulling from dockerhub"
                    }
                }
            }
        }
        
        stage("SonarQube Code Analysis") {
            steps {
                withSonarQubeEnv("Sonar") {
                    sh """
                    $SONAR_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=dify \
                    -Dsonar.projectKey=dify \
                    -Dsonar.sources=. \
                    -Dsonar.exclusions=**/node_modules/**,**/*.test.js,**/vendor/**,**/*.md
                    """
                }
            }
        }

        stage("Trivy File System Scan") {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }

        stage('Setup Namespaces') {
            steps {
                script {
                    def namespaces = ['dev', 'prod']

                    namespaces.each { ns ->
                        echo "Checking namespace: ${ns}"

                        def nsExists = sh(
                            script: "kubectl get namespace ${ns}",
                            returnStatus: true
                        )

                        if (nsExists == 0) {
                            echo "Namespace '${ns}' already exists. Skipping creation."
                        } else {
                            echo "Namespace '${ns}' does not exist. Creating..."
                            sh "kubectl create namespace ${ns}"
                        }
                    }
                }
            }
        }
        stage('Verify Namespaces before deployment') {
            steps {
                script {
                    echo "Listing all namespaces"
                    sh "kubectl get ns"
                }
            }
        }

        stage("Deployment Stage") {
            steps {
                script {
                    def namespace = params.DEPLOY_ENV
                    echo "Deploying to namespace: ${namespace}"

                    def helmPath = 'Helm/charts/dify'
                    def releaseName = 'dify'

                    def releaseExists = sh(script: "helm status ${releaseName} -n ${namespace}", returnStatus: true) == 0

                    if (releaseExists) {
                        sh "helm upgrade --install ${releaseName} ${helmPath} -n ${namespace}"
                    } else {
                        sh "helm repo add dify https://borispolonsky.github.io/dify-helm"
                        sh "helm repo update"
                        sh "helm upgrade --install release dify/dify -n ${namespace} --dry-run --debug"
                        sh "helm upgrade --install release dify/dify -n ${namespace}"
                    }
                }
            }
        }

    }
}
