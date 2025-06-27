pipeline {
    agent { label 'tarek-agent'}

    environment {
        DOCKER_IMAGE = 'tareqmohamed/posts_proj'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                withCredentials([gitUsernamePassword(credentialsId: '1',
                gitToolName: 'git-tool')]) {
                    sh "git checkout main"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE:${IMAGE_TAG} ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '2', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push $DOCKER_IMAGE:${IMAGE_TAG}"
            }
        }

        stage('Run ansible to update tag in k8s file') {
          steps {
                sh """
                    ansible-playbook update-tag.yml -e new_tag=${IMAGE_TAG}
                """
            }
            
        }
        stage('Commit and Push Changes') {
            steps {
                    withCredentials([gitUsernamePassword(credentialsId: '1',
                 gitToolName: 'git-tool')]) {
            sh """
        git config user.name "tarek mohamed"
        git config user.email "telmagic10@gmail.com"
        git add k8s/app.yml
        git commit -m "Update image tag to ${IMAGE_TAG} [ci skip]" || echo "No changes"
        git push
    """
}

            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }
}
