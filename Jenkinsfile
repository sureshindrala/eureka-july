pipeline {
    agent {
        label 'k8s-slave'
    }
    environment {
        APPLICATION_NAME = "eureka"
    }
    stages {
        stage('build') {
            steps{
                echo "**********priniting ${env.APPLICATION_NAME}****************"
                sh "mvn clean package -DskipTests=true "
                archive 'target/*.jar'

            }
        }
        stage ('sonar') {
            echo "******************Sonar stage***************"
            sh """
                mvn clean verify sonar:sonar \
                -Dsonar.projectKey=chathura-eureka \
                -Dsonar.host.url=http://35.231.155.151:9000 \
                -Dsonar.login=sqa_c80e3244593c78df97164386bc32a4638fae0ae1
            """            
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