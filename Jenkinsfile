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
          junit "target/surfire-reports/*.xml" 
        }
      }
      stage("build"){
        steps{
          sh "mvn clean package -DskipTests"
        }
      }
    }
}
