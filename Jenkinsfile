pipeline {
    agent {
        label 'k8s-slave'
    }
    tools {
        
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
            steps {
            echo "******************Sonar stage***************"
            sh """
                mvn sonar:sonar \
                -Dsonar.projectKey=maven_project \
                -Dsonar.host.url=http://35.196.58.210:9000 \
                -Dsonar.login=sqa_14f849b53741e77bf3dfaa31d6f061d200acd1e5
            """            
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