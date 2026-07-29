pipeline {
    agent {
        label 'k8s-slave'
    }
    stages {
        stage('build') {
            steps{
                echo "priniting eureka applications"
                sh "mvn clean package -DskipTests=true "
                artifacts: 'target/*jar'

            }
        }
    }
    
}

//  @Library("com.chathura.slb@main") _ 
//  jfrogPipeline (
//      appName: 'eureka'

//  )


//  @Library("com.chathura.slb@main") _ 
//  k8sPipeline (
//      appName: 'eureka'

//  )




// @Library("com.chathura.slb@main") _ 
// dockerPipeline (
//     appName: 'eureka',
//     devHostPort: '5761',
//     contPort: '8761'
// )
// // L shoud be capital