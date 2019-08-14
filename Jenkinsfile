pipeline {
    agent {
            docker {
                image 'maven:3-alpine'
                args '-v /root/.m2:/root/.m2'
            }
        }
    //tools {
      ///  maven 'Maven 3.3.9'
     //   jdk 'jdk8'
  //  }


     parameters {


        // publishDoc stage is deliberately disabled, because it triggers 'gradlew html2Confluence' which do not works with current gradle version 4.3
        // When gradle is successfully upgraded to 5.5.1+ it can be enabled.

       // string name: 'agentLabel', defaultValue: 'kubernetes', description: 'Build Agent Label', trim: true
        string name: 'agentWorkspace', defaultValue: 'microservices-demo', description: 'Workspace on the Build Agent', trim: true
        string name: 'gitURI', defaultValue: 'ssh://git@github.com:mehdimousavi9/microservices-demo.git', description: 'Git URI', trim: true
        string name: 'gitBranch', defaultValue: 'master', description: 'Git Branch to Checkout', trim: true

        /*string name: 'buildTasks', defaultValue: 'build test testIntegration --continue', description: 'Tasks to be executed in the Gradle Build stage', trim: true
        string name: 'buildOptions', defaultValue: '-PpreReleaseQualifier=-rc${BUILD_NUMBER} -PebStatus=release-noneban', description: 'Options to be passed to Gradle Build tasks', trim: true
        string name: 'emailRecipients', defaultValue: '', description: 'Space-separated list of email recipients for sending build status notifications', trim: true
        string name: 'dockerRegistry', defaultValue: 'ebs-v-registry02.ebs.crealogix.net:443', description: 'Docker registry for pushing the image', trim: true
        string name: 'dockerUser', defaultValue: 'test', description: 'Docker registry username', trim: true
        string name: 'dockerPass', defaultValue: 'test', description: 'Docker registry password', trim: true
        string name: 'dockerContainerLabel', defaultValue: 'mdb-master-latboot', description: 'Cloud environment value that is used for service name, config server and configuration (e.g. mdb-master-latboot)', trim: true
        string name: 'dbhModuleName', defaultValue: "digipass", description: 'Name of the DBH module', trim: true

        string name: 'dockerHost', defaultValue: 'tcp://ebs-v-dock11.ctr.ebs.crealogix.net:2375', description: 'Remote Docker Host'
        string name: 'dockerRegistry', defaultValue: 'test-docker.repo-jev.int.crealogix-online.com', description: 'Docker Registry'
        string name: 'dockerRegistryCredentialsId', defaultValue: 'docker-registry-jever', description: 'Docker Registry Credentials ID'
        string name: 'dockerContainerLabel', defaultValue: 'mdb-master-latboot', description: 'Cloud environment value that is used for service name, config server and configuration (e.g. mdb-master-latboot)', trim: true

        string name: 'helmRegistry', defaultValue: 'https://repo-jev.int.crealogix-online.com/artifactory/virtual-test-helm/', description: 'Helm chart repository', trim: true
        string name: 'helmRegistryCredentialsId', defaultValue: 'artifactory-jev-upload-user', description: 'Helm registry credentials id', trim: true
        string name: 'k8sClusterCredentialsId', defaultValue: 'kube-config-file', description: 'Kubernetes cluster credentials id', trim: true
        string name: 'k8sClusterUrl', defaultValue: 'https://gceo-v-rancher01/k8s/clusters/c-qzbd9', description: 'Kubernetes cluster URL', trim: true
        string name: 'k8sNamespace', defaultValue: 'mdb-master-lat', description: 'Environment / release / namespace name', trim: true
        string name: 'helmOptions', defaultValue: '--wait', description: 'Additional chart installation/upgrade param (ie. for upgrade: --recreate-pods)', trim: true
*/
        string name: 'PULL_REQUEST_URL', description: 'Pull request URL', trim: true
        string name: 'PULL_REQUEST_ID', description: 'Pull request identifier', trim: true
        string name: 'PULL_REQUEST_TO_BRANCH', description: 'Pull request target branch', trim: true
        string name: 'PULL_REQUEST_USER_EMAIL_ADDRESS', description: 'Pull request user email address', trim: true

      }



    stages {
        /*stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }*/

        stage("Checkout") {
            when {
                expression { env.checkout == "true" }
            }
            steps {
                script {
                  //notifyBitbucket()
                  currentBuild.description = "<a href='${params.PULL_REQUEST_URL}'>PR #${params.PULL_REQUEST_ID} - ${params.PULL_REQUEST_USER_EMAIL_ADDRESS}</a>"
                  checkout ([$class: 'GitSCM', branches: [[name: "${gitBranch}"]], userRemoteConfigs: [[url: "${gitURI}", credentialsId: "${SSH_ID_MAHDI_TEST}"]] ])
                }
                sh "chmod +x gradlew"
            }
        }

       /* stage ('Build') {
            steps {
                sh 'mvn -D maven.test.failure.ignore=true install'
            }
            post {
                success {
                    junit 'target/surefire-reports/** /*.xml'
                }
            }
        }*/

        stage('Build') {
                    steps {
                        sh 'mvn -B -DskipTests clean package'
                    }
         }
         stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                 }
            }
         }
         //stage('Deploy - Staging') {
            // steps {
             //    sh './deploy staging'
            //     sh './run-smoke-tests'
            // }
         //}
        /*stage('Deploy') {
                    when {
                      expression {
                        currentBuild.result == null || currentBuild.result == 'SUCCESS'
                      }
                    }
                    steps {
                        sh 'mvn deploy'
                    }
                }*/

         stage('Deliver') {
            steps {
                 sh 'ifconfig'
                 sh 'java -jar target/microservices-demo-2.0.0.RELEASE.jar'

            }
         }
    }
}