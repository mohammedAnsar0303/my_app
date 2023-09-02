pipeline {
    agent any
    environment {
        TAG = "v0.${env.BUILD_NUMBER}"
    }
    
    stages {
         stage('Compile-Package') {
            steps {
                script {
                    def mvnHome = tool name: 'maven3', type: 'maven'
                    sh "${mvnHome}/bin/mvn clean package"
                    sh 'mv target/myweb*.war:${TAG} target/newapp.war'
                }
            }
        }
	    
    // stage('Upload to S3') {
    //   steps {
	// sh '''
    //       AWS_ACCESS_KEY_ID='aws-credentials'
    //       AWS_SECRET_ACCESS_KEY='aws-credentials'
    //       AWS_DEFAULT_REGION=us-east-1
    //       aws s3 cp target/*.war s3://my-project-artifacts-2023/
    //         '''
    //   	}
    //   }
	    
	// stage('SonarQube Analysis') {
    //         steps {
    //             script {
    //                 def mvnHome = tool name: 'maven3', type: 'maven'
    //                 withSonarQubeEnv('sonar') {
    //                     sh "${mvnHome}/bin/mvn sonar:sonar"
    //                 }
    //             }
    //         }
    //     }

    //     stage('Build Docker Image') {
    //         steps {
    //             sh 'docker build -t mlogu6/myweb:${TAG} .'
    //         }
    //     }

    //     stage('Docker Image Push') {
    //         steps {
    //             withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
    //                 sh "docker login -u mlogu6 -p ${dockerPassword}"
    //             }
    //             sh 'docker push mlogu6/myweb:${TAG}'
    //         }
    //     }

	// stage('Delete Docker Images') {
    //         steps {
    //             sh 'docker image prune --all --force'
    //         }
    //     }

	// stage('Ansible Deployment') {
    //         steps {
	// 	script {
    //             ansiblePlaybook credentialsId: 'my_project', disableHostKeyChecking: true, extras: "-e DOCKER_TAG=${TAG}", installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy.yml'
    //         }
	//   }
    //     }
    }
}
