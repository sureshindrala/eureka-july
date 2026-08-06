pipeline {
    agent {
        label 'k8s-slave'
    }
    tools {
        jdk 'java_21'
        maven 'maven_3.6'
    }
    environment {
        APPLICATION_NAME = "eureka"
        SONAR_HOST = "http://35.196.58.210:9000"
        POM_VERSION = readMavenPom().getVersion()
        POM_PACKAGING = readMavenPom().getPackaging()
        DOCKER_HUB = "docker.io/sureshindrala"
        DOCKER_CREDS = credentials('docker_creds')
        

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
             echo "******************${APPLICATION_NAME}-*****Sonar stage*******************"
                withCredentials([string(credentialsId: 'sonar_creds', variable: 'sonar_creds')]) {
                    sh """
                        mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar\
                        -Dsonar.projectKey=maven_project \
                        -Dsonar.host.url=$SONAR_HOST\
                        -Dsonar.login=$sonar_creds
                    """ 
                }
           
            }
            
        }
        stage ('BUILD_FORMAT') {
            steps {

                script {
                    echo "Testing JAR SOURCE: chathura-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING}"
                    echo "Testing Destination Format: chathura-${env.APPLICATION_NAME}-${currentBuild.number}-${BRANCH_NAME}.${env.POM_PACKAGING}"


                }

            }
        }
        // stage ('Docker_BUILD') {
        //     steps {
        //         script {
        //             echo "************Build Docker Image*****************"
        //             sh "cp ${workspace}/target/chathura-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd"
        //             sh "ls -la ./.cicd"
        //             sh "docker build --force-rm --no-cache --pull --rm=true --build-arg JAR_SOURCE=chathura-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:${GIT_COMMIT} ./.cicd "
            
        //         }
        //     }
        // }
        stage ('Docker Push') {
            steps {
            script {
            echo "*****************building Docker image***********************"
                sh """
                    cp ${workspace}/target/chathura-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} ./.cicd
                    ls -la ./.cicd
                    docker build --force-rm --no-cache --pull --rm=true --build-arg JAR_SOURCE=chathura-${env.APPLICATION_NAME}-${env.POM_VERSION}.${env.POM_PACKAGING} -t ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:${GIT_COMMIT}  ./.cicd
                    echo "***********Docker login***********************"
                    echo "Username: $DOCKER_CREDS_USR"
                    docker login -u ${DOCKER_CREDS_USR} -p ${DOCKER_CREDS_PSW} 
                    docker push ${env.DOCKER_HUB}/${env.APPLICATION_NAME}:${GIT_COMMIT}


                """                     
                }
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