//user services
pipeline {
  agent {    
       kubernetes {
       defaultContainer 'dind-slave'  
       yaml """
      apiVersion: v1 
      kind: Pod 
      metadata: 
          name: k8s-worker
      spec: 
          containers: 
            - name: dind-slave
              image: docker:1.12.6-dind 
              resources: 
                  requests: 
                      cpu: 20m 
                      memory: 512Mi 
              securityContext: 
                  privileged: true 
              volumeMounts: 
                - name: docker-graph-storage 
                  mountPath: /var/lib/docker
            - name: kube-helm-slave
              image:  qayesodot/slave-jenkins:kube-helm
              command: ["/bin/sh"]
              args: ["-c","while true; do echo hello; sleep 10;done"]            
          volumes: 
            - name: docker-graph-storage 
              emptyDir: {}
 """
    }
  }
    stages {
      //  this stage create enviroment variable from git for discored massage
    //   stage('get_commit_msg') {
    //     steps {
    //       container('jnlp'){
    //       script {
    //         env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
    //         env.GIT_SHORT_COMMIT = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
    //         env.GIT_COMMITTER_EMAIL = sh (script: "git --no-pager show -s --format='%ae'", returnStdout: true  ).trim()
    //         env.GIT_REPO_NAME = scm.getUserRemoteConfigs()[0].getUrl().tokenize('/')[3].split("\\.")[0]
            
    //         // Takes the branch name and replaces the slashes with the %2F mark 
    //         env.BRANCH_FOR_URL = sh([script: "echo ${GIT_BRANCH} | sed 's;/;%2F;g'", returnStdout: true]).trim()
    //         // Takes the job path variable and replaces the slashes with the %2F mark 
    //         env.JOB_PATH = sh([script: "echo ${JOB_NAME} | sed 's;/;%2F;g'", returnStdout: true]).trim()
    //         // creating variable that contain the job path without the branch name  
    //         env.JOB_WITHOUT_BRANCH = sh([script: "echo ${env.JOB_PATH} | sed 's;${BRANCH_FOR_URL};'';g'", returnStdout: true]).trim() 
    //         //  creating variable that contain the JOB_WITHOUT_BRANCH variable without the last 3 characters 
    //         env.JOB_FOR_URL = sh([script: "echo ${JOB_WITHOUT_BRANCH}|rev | cut -c 4- | rev", returnStdout: true]).trim()  
    //         echo "${env.JOB_FOR_URL}"
    //       }
    //     }
    //   }
    // }
    //   stage('create nameSpace and configMap in the cluster') {
    //     // when {
    //     //   anyOf {
    //     //     branch 'master'; branch 'develop'
    //     //   }
    //     // }
    //     steps {
    //       container('kube-helm-slave'){
    //         sh("kubectl get ns develop || kubectl create ns develop")
    //         // sh("kubectl get ns ${env.BRANCH_NAME} || kubectl create ns ${env.BRANCH_NAME}")
    //         sleep(10)
    //       script {
    //         if(env.BRANCH_NAME == 'devops/cis') {
    //           configFileProvider([configFile(fileId:'34e71bc6-8b5d-4e31-8d6e-92d991802dcb',variable:'MASTER_CONFIG_FILE')]){
    //           sh ("kubectl get cm kd.config --namespace master|| kubectl apply -f ${env.MASTER_CONFIG_FILE}")
    //           sh 'helm list'
    //           // sh ("kubectl get cm kd.config --namespace ${env.BRANCH_NAME} || kubectl apply -f ${env.MASTER_CONFIG_FILE}")  
    //           }    
    //         }
    //         else{
    //           configFileProvider([configFile(fileId:'abda1ce7-3925-4759-88a7-5163bdb44382',variable:'DEVELOP_CONFIG_FILE')]){
    //             sh ("kubectl get cm kd.config --namespace develop || kubectl apply -f ${env.DEVELOP_CONFIG_FILE}")
    //             //sh ("kubectl get cm kd.config --namespace ${env.BRANCH_NAME} || kubectl apply -f ${env.DEVELOP_CONFIG_FILE}")  
    //           }
    //         }
    //       }
    //     }
    //   }
    // }

    stage('clone kd-helm reposetory and update the tag '){
      // when {
      //   anyOf {
      //     branch 'master'; branch 'develop'
      //   }
      // }
      steps {
         container('jnlp'){
          git branch: 'master',
            credentialsId: 'gitHubToken',
            url: 'https://github.com/meateam/kd-helm.git'
            sh 'cat common/templates/_deployment.yaml'
        script {
            env.space1 = "        "
            env.space2 = "      "
            env.IMAGE_PULL_SECRETS ='sed -i "imagePullPolicy: {{ .Values.image.pullPolicy }}/          imagePullPolicy: {{ .Values.image.pullPolicy }}"\n"      imagePullSecrets:"\n"        - name: acr-secret/g" ./common/templates/_deployment.yaml'
        }
           sh "sed -i.bak '29 ${env.space2} i imagePullSecrets:' ./common/templates/_deployment.yaml"
          //  sh "sed '30 ${env.space1} name: acr-secret' ./common/templates/_deployment.yaml"
           sh "sed -i.bak '30 i ${env.space1} i '-' name: acr-secret' ./common/templates/_deployment.yaml"

        // sh "echo ${env.IMAGE_PULL_SECRETS} > changeCommonDeployments.sh"
        // sh "chmod 755 changeCommonDeployments.sh"
        // sh "ls"
        // sh "cat ./changeCommonDeployments.sh"
        // sh "./changeCommonDeployments.sh"
            // sh 'sed -i `s;imagePullPolicy: {{ .Values.image.pullPolicy }};          imagePullPolicy: {{ .Values.image.pullPolicy }}\n      imagePullSecrets:\n        - name: acr-secret;g` common/templates/_deployment.yaml'
            // sh([script: "sed -i 's;imagePullPolicy: {{ .Values.image.pullPolicy }};          imagePullPolicy: {{ .Values.image.pullPolicy }}\n      imagePullSecrets:\n        - name: acr-secret;g' common/templates/_deployment.yaml"])
            // sh "sed -i 's;imagePullPolicy: {{ .Values.image.pullPolicy }};${env.IMAGE_PULL_SECRETS};g' common/templates/_deployment.yaml"
            //sh([script: "sed -i -e 's;imagePullPolicy:;${env.IMAGE_PULL_SECRETS};g'"]) 
            sh 'cat common/templates/_deployment.yaml'
            //sh "rm ./changeCommonDeployments.sh"
         }
      }
    }

      // build image for unit test 
      // stage('build dockerfile of tests chara') {
      //   steps {
      //       sh "docker build -t unittest -f test.Dockerfile ." 
      //   }  
      // }
      // run image of unit test
    //   stage('run unit tests') {   
    //     steps {
    //         sh "docker run unittest"  
    //     }
    //     post {
    //       always {
    //         discordSend description: '**service**: '+ env.GIT_REPO_NAME + '\n **Build**:' + " " + env.BUILD_NUMBER + '\n **Branch**:' + " " + env.GIT_BRANCH + '\n **Status**:' + " " +  currentBuild.result + '\n \n \n **Commit ID**:'+ " " + env.GIT_SHORT_COMMIT + '\n **commit massage**:' + " " + env.GIT_COMMIT_MSG + '\n **commit email**:' + " " + env.GIT_COMMITTER_EMAIL, footer: '', image: '', link: 'http://jnk-devops-ci-cd.northeurope.cloudapp.azure.com/blue/organizations/jenkins/'+env.JOB_FOR_URL+'/detail/'+env.BRANCH_FOR_URL+'/'+env.BUILD_NUMBER+'/pipeline', result: currentBuild.result, thumbnail: '', title: 'link to logs of unit test', webhookURL: env.discord   
    //       }
    //     }
    //   }
    //   // login to acr when pushed to branch master or develop
    //   stage('login to azure container registry') {
    //     when {
    //       anyOf {
    //         branch 'master'; branch 'develop'
    //       }
    //     }
    //     steps{  
    //       withCredentials([usernamePassword(credentialsId:'DRIVE_ACR',usernameVariable: 'USER', passwordVariable: 'PASS')]) {
    //         sh "docker login  drivehub.azurecr.io -u ${USER} -p ${PASS}"
    //       }
    //     }
    //   }
    //   // when pushed to master or develop build image and push to acr     
    //   stage('build dockerfile of system only for master and develop and push them to acr') {
    //     when {
    //       anyOf {
    //          branch 'master'; branch 'develop'
    //       }
    //     }
    //     steps {
    //       script{
    //        if(env.GIT_BRANCH == 'master') {
    //           sh "docker build -t  drivehub.azurecr.io/${env.GIT_REPO_NAME}:master_${env.GIT_SHORT_COMMIT} ."
    //           sh "docker push  drivehub.azurecr.io/${env.GIT_REPO_NAME}:master_${env.GIT_SHORT_COMMIT}"
    //         }
    //         else if(env.GIT_BRANCH == 'develop') {
    //            sh "docker build -t  drivehub.azurecr.io/${env.GIT_REPO_NAME}:develop ."
    //           sh "docker push  drivehub.azurecr.io/${env.GIT_REPO_NAME}:develop"  
    //         }
    //       } 
    //     }
    //     post {
    //       always {
    //         discordSend description: '**service**: '+ env.GIT_REPO_NAME + '\n **Build**:' + " " + env.BUILD_NUMBER + '\n **Branch**:' + " " + env.GIT_BRANCH + '\n **Status**:' + " " +  currentBuild.result + '\n \n \n **Commit ID**:'+ " " + env.GIT_SHORT_COMMIT + '\n **commit massage**:' + " " + env.GIT_COMMIT_MSG + '\n **commit email**:' + " " + env.GIT_COMMITTER_EMAIL, footer: '', image: '', link: 'http://jnk-devops-ci-cd.northeurope.cloudapp.azure.com/blue/organizations/jenkins/'+env.JOB_FOR_URL+'/detail/'+env.BRANCH_FOR_URL+'/'+env.BUILD_NUMBER+'/pipeline', result: currentBuild.result, thumbnail: '', title:'Logs build dockerfile master/develop', webhookURL: env.discord   
    //       }
    //     } 
    //   }      
    }
        
}