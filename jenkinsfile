def versionPom = ""
pipeline{
  agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: chikitor/jenkins-nodo-java-bootcamp:1.0
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-socket-volume
    securityContext:
    privileged: true
  volumes:
  - name: docker-socket-volume
    hostPath:
      path: /var/run/docker.sock
      type: Socket
    command:
    - sleep
    args:
    - infinity
  '''
         defaultContainer 'shell'
        }
    }
  //environment {
  //      NEXUS_VERSION = "nexus3"
  //      NEXUS_PROTOCOL = "http"
  //      NEXUS_URL = "192.168.67.3:8081"
  //      NEXUS_REPOSITORY = "bootcamp"
  //      NEXUS_CREDENTIAL_ID = "Ferdevcenter" 
  //      DOCKERHUB_CREDENTIALS=credentials("Ferdevcenterdockerhub")
  //      DOCKER_IMAGE_NAME="chikitor/spring-boot-app" 
  // }

    environment {
        registryCredential='Ferdevcenterdockerhub'
        registryBackend = 'chikitor/spring-boot-app'
    }

    stages{ 

      stage("build"){
        steps{
          sh "mvn clean package -DskipTests"
        }
      }

        stage('Push Image to Docker Hub') {
          steps {
            script {
              dockerImage = docker.build registryBackend + ":$BUILD_NUMBER"
              docker.withRegistry( '', registryCredential) {
                dockerImage.push()
              }
            }
          }
        }

        stage('Push Image latest to Docker Hub') {
          steps {
            script {
              dockerImage = docker.build registryBackend + ":latest"
              docker.withRegistry( '', registryCredential) {
                dockerImage.push()
              }
            }
          }
        }
   
      stage("deploy to k8s") {
            steps{
                sh "git clone https://github.com/Ferdevcenter/kubernetes-helm-docker-config.git configuracion --branch test-implementation"
                sh "kubectl apply -f configuracion/kubernetes-deployments/spring-boot-app/manifest.yaml --kubeconfig=configuracion/kubernetes-config/config"
            }
      }
    }
}