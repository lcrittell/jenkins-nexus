
pipeline {
    agent any

    environment {
        APP_VERSION = "${BUILD_ID}"
        NEXUS_REGISTRY = "nexus.udemy-demo.online:5443"
    }

    stages {
        stage('Build and Push Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'my-nexus',
                        usernameVariable: 'NEXUS_USERNAME',
                        passwordVariable: 'NEXUS_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$NEXUS_PASSWORD" | docker login nexus.udemy-demo.online:5443 \
                            -u "$NEXUS_USERNAME" --password-stdin

                        docker build --build-arg APP_VERSION="$APP_VERSION" \
                            -t nexus.udemy-demo.online:5443/app:$APP_VERSION .

                        docker push nexus.udemy-demo.online:5443/app:$APP_VERSION

                        docker logout nexus.udemy-demo.online:5443
                    '''
                }
            }
        }
    }
}
