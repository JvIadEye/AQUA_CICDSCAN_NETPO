def gv_script
pipeline{
    agent any
    stages{
        stage('Init'){
            steps{
                script{
                    gv_script = load "script.groovy"
                }
            }
        }
        
        stage('Build'){
            steps{
                script{
                    gv_script.buildApp()
                }
            }
        }

        stage('Test'){
            steps{
                script{
                    gv_script.testApp()
                }
            }
        }
        
        stage('Push image'){
            steps{
                script{
                    gv_script.pushImage()
                }
            }
        }

        stage('Deploy'){
            steps{
                script{
                    gv_script.deployApp()
                }
            }
        }

        stage('AQUA CICD scanning'){
            steps{
                script{
                    //withCredentials([usernamePassword(credentialsId: 'nexus', passwordVariable: 'PSW', usernameVariable: 'USER')]){
                    //sh '''
                    //    docker login https://registry.aquasec.com -u ${USER} -p ${PSW}
                    //    docker pull 192.168.1.231:9006/dockerhosted-repo:4
                    //    docker logout
                    //'''
                    //}
                    
                    withCredentials([usernamePassword(credentialsId: 'AquaCloud', passwordVariable: 'PSW', usernameVariable: 'USER')]){
                    sh '''
                        docker login https://registry.aquasec.com -u huy.tran@netpoleons.com -p admin123
                        docker pull registry.aquasec.com/scanner:2022.4.557
                        docker pull 172.16.2.74:9006/dockerhosted-repo:${BUILD_NUMBER}
                        docker logout

                        docker run -e BUILD_JOB_NAME=${JOB_NAME} -e BUILD_URL=${BUILD_URL} -e BUILD_NUMBER=${BUILD_NUMBER} --rm -v /var/run/docker.sock:/var/run/docker.sock registry.aquasec.com/scanner:2022.4.557 scan --token f8211857-3864-426b-ab01-ffb7692172c2 --host https://630d27b67d.cloud.aquasec.com --local 172.16.2.74:9006/dockerhosted-repo:${BUILD_NUMBER} --checkonly --no-verify --layer-vulnerabilities                    
                    '''
                    }
                }
            }
        }
    }
    post{
        success{
            sh 'echo "Pipeline created successfully!"'
        }
    }
}
