pipeline {
    agent any

    stages{

        stage('Run Unit Tests'){
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image'){
            steps {
                sh "docker build -t abdelrahmanelhelw/ivolve-lab22:$BUILD_NUMBER ."
            }
        }

        stage('Push Image to DockerHub'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                        docker push abdelrahmanelhelw/ivolve-lab22:$BUILD_NUMBER
                    '''
                }
            }
        }

        stage('Delete local image'){
            steps {
                sh " docker rmi abdelrahmanelhelw/ivolve-lab22:$BUILD_NUMBER"
            }
        }

        stage('update deployment file'){
            steps {
                sh "sed -i 's|image: .*|image: abdelrahmanelhelw/ivolve-lab22:$BUILD_NUMBER|' deployment.yaml"
            }
        }



        // stage('Deploy to Kubernets'){
        //     steps {
        //         withCredentials([file(credentialsId: 'kubeconfig' , variable: 'KUBECONFIG')]) {
        //             sh '''
        //                 export KUBECONFIG=$KUBECONFIG
        //                 kubectl apply -f deployment.yaml
        //             '''
                   
        //     }

        // }
        // }

        stage('Deploy to Kubernets'){
            steps {
                withCredentials([string(credentialsId: 'Kubernets', variable: 'TOKEN')]) {
                    sh " kubectl apply -f deployment.yaml --server=https://127.0.0.1:32771 --token=$TOKEN --insecure-skip-tls-verify=true --validate=false"
                }
            }
        }

    }

    post { 
        always {
            echo 'pipeline completed! '
        }
        success {
            echo 'Pipeline completed successfully!, Used image with tag ${BUILD_NUMBER}'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
