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
          jacoco()
          junit "target/surfiregit-reports/*.xml" 
        }
      }
      stage("build"){
        steps{
          sh "mvn clean package -DskipTests"
        }
      }
    }
}
