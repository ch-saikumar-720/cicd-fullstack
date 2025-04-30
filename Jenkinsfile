pipeline {
  agent any

  environment {
    AWS_REGION = 'us-east-1'
    ECR_REGISTRY = '211125549407.dkr.ecr.us-east-1.amazonaws.com'
    REPO_NAME = 'kubes'
    CLUSTER_NAME = 'kubes'
    BACKEND_IMAGE = "${ECR_REGISTRY}/${REPO_NAME}/backend:latest"
    FRONTEND_IMAGE = "${ECR_REGISTRY}/${REPO_NAME}/frontend:latest"
    KUBECONFIG = '/var/lib/jenkins/.kube/config'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Login to ECR') {
      steps {
        sh '''
          aws ecr get-login-password --region $AWS_REGION | \
          docker login --username AWS --password-stdin $ECR_REGISTRY
        '''
      }
    }

    stage('Build and Push Backend') {
      steps {
        dir('backend') {
          script {
            // Check if ECR repository 'kubes' exists, create if it doesn't
            sh '''
              aws ecr describe-repositories --repository-names kubes --region $AWS_REGION || \
              aws ecr create-repository --repository-name kubes/backend --region $AWS_REGION
            '''
            // Build and push the backend image
            sh '''
              docker build -t $BACKEND_IMAGE .
              docker push $BACKEND_IMAGE
            '''
          }
        }
      }
    }

    stage('Update Kubeconfig') {
      steps {
        sh '''
          aws eks --region $AWS_REGION update-kubeconfig --name $CLUSTER_NAME
        '''
      }
    }

    stage('Deploy MySQL and Backend') {
      steps {
        script {
          // Update the backend.yaml with the latest image name
          sh """
            sed -i 's|image: .*|image: ${BACKEND_IMAGE}|g' backend/backend.yaml
          """
          // Apply the MySQL and backend deployment
          sh '''
            kubectl apply -f backend/mysql.yaml
            kubectl apply -f backend/backend.yaml
          '''
        }
      }
    }

    stage('Wait for Backend LB') {
      steps {
        script {
          def lb_dns = ''
          timeout(time: 3, unit: 'MINUTES') {
            waitUntil {
              lb_dns = sh(
                script: "kubectl get svc backend-service -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'",
                returnStdout: true
              ).trim()
              return lb_dns != ''
            }
          }

          echo "Backend LoadBalancer DNS: ${lb_dns}"

          // Update frontend JS files with the backend LoadBalancer DNS
          sh """
            sed -i "s|http://.*:5000|http://${lb_dns}:5000|g" frontend/src/login.js
            sed -i "s|http://.*:5000|http://${lb_dns}:5000|g" frontend/src/signup.js
          """
        }
      }
    }

    stage('Build and Push Frontend') {
      steps {
        dir('frontend') {
          sh '''
            docker build -t $FRONTEND_IMAGE .
            docker push $FRONTEND_IMAGE
          '''
        }
      }
    }

    stage('Deploy Frontend') {
      steps {
        sh '''
          kubectl apply -f frontend/frontend.yaml
        '''
      }
    }

    stage('Test K8s Access') {
      steps {
        sh 'kubectl get nodes'
      }
    }
  }
}
