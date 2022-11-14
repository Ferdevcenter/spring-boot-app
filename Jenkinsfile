pipeline{
  agent{
    node {
      label "nodo-java"
    }
  }
    stages{
      stage("test"){
        steps{
          sh "mvn test"
          junit "target/test-reports/*.xml" 
        }
      }
      stage("build"){
        steps{
          sh "mvn clean package -DskipTests"
        }
      }
    }
}
