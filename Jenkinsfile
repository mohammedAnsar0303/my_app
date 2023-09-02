pipeline {
    agent any
    environment {
        TAG = "v0.${env.BUILD_NUMBER}"
        S3_BUCKET = "myproject-artifacts"
    }
    
    stages {
        // install maven bianary and add the path in manage jenkins > tools after install the maven plugin 
         stage('Compile-Package') {
            steps {
                script {
                    def mvnHome = tool name: 'maven3', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean package"
                    sh 'mv target/myweb*.war target/newapp.war'
                }
            }
        }
	    
    stage('Upload to S3') {
        // create a iam role and add it to the jenkins server to access the aws
      steps {
	sh '''
          AWS_ACCESS_KEY_ID='aws-credentials'
          AWS_SECRET_ACCESS_KEY='aws-credentials'
          AWS_DEFAULT_REGION=ap-south-1
          aws s3 cp target/*.war s3://${S3_BUCKET}/newapp-${TAG}.war
            '''
      	}
      }
	    
	stage('SonarQube Analysis') {
        // generate a token in sonarqube console and add it in the jenkins credentials
            steps {
                script {
                    def mvnHome = tool name: 'maven3', type: 'maven'
                    withSonarQubeEnv('sonar') {
                        sh "${mvnHome}/bin/mvn sonar:sonar"
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // install docker in the jenkins server
                // execute this command to give the permission to build the image "chmod 777 /var/run/docker.sock"
                sh 'docker build -t mlogu6/myweb:${TAG} .'
            }
        }

        stage('Docker Image Push') {
            steps {
                withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
                    sh "docker login -u mlogu6 -p ${dockerPassword}"
                }
                sh 'docker push mlogu6/myweb:${TAG}'
            }
        }

        stage (Deployment) {
            steps {
                sh 'docker run -d -p 8091:8080 --name myproject mlogu6/myweb:${TAG}'
            }
        }
	    // stage('Delete Docker Images') {
        //     steps {
        //         sh 'docker image prune --all --force'
        //     }
        // }

	//     stage('Ansible Deployment') {
    //         steps {
	// 	script {
    //             ansiblePlaybook credentialsId: 'my_project', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy.yml'
    //         }
	//   }
    //     }
    }
}

