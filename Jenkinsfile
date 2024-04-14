pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "debatradevelop/train-schedule"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE_NAME)
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker_hub_login') {
                        docker.image(env.DOCKER_IMAGE_NAME).push("${env.BUILD_NUMBER}")
                        docker.image(env.DOCKER_IMAGE_NAME).push("latest")
                    }
                }
            }
        }
        stage('Canary Deploy') {
            environment { 
                CANARY_REPLICAS = 1
            }
            steps {
                script {
                    def kubeConfig = """\
                        apiVersion: v1
                        clusters:
                        - cluster:
                            certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJME1ETXlNREUzTkRVd04xb1hEVE0wTURNeE9ERTNORFV3TjFvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTGl1ClFDaDFORVVHU3R6bG05bEx1MVRwUWZaMTZ4RGZzK2YxTEFKT3EzRERobVYvb1Y4Z3ZTQkpqSHR6ZHN3TnVOa20Kd2JJbitQMEF5Q3lkd0tiUlJjVGpuRExpYytBY2xubmZ5cnFheHpuMExLUGNmUjBpR0laanhjLzlZUmFsSERoTwoxVGk3ak9lanl2eUM5SGZnWHU2SVk0U0l5ajM3MEh4dEFqTk80QWM3dkFPZllqNVZEWlBKYVF0SmQ1dHBsWG8wClZCUDF1L1N0MjBOTkxXTnZvQlQxd1IrVjQ1Nyt2MngrMjQxSXM3eFVvUGlaUnRkaERIdHl4aDNvYnBJcUpPUXYKcGQ5TzVBSXdvSkJCN1NVSUE5UlQyU3dmUnNaN3dxK2F6TGRKVDBJTWxlVnhNU3dSVno3UlRKWnlXMSs4M3lZbgpMWit6YUIzUEpab2pwbTNnOXNjQ0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFEcXAwV2xCam9IRktBMVd2UkdBOXlzcUFmSFQKTm1ObEFIbk8wUkpOaTZNU2NacjMzNzRCUm04Uk9IL0M0ZE1oeDBkNnA0VUgyYWVWWDBoS0pMZEZIWFBFTmRKMwppRXVDV25icy95SXJOLzMwc0Zxa2E2bk9Wb2IwNXZLSFJJMmlLdms2V3Z3cUZ2WmM3cjJOays4SDVYc25IeWczCm5NaEw3RGd2c0JwUzhUQ2ZTcVQrTzh0SWh0bURodFY4bksremVqYmt2eXFOaDdMNndvd0NIcDgrV2tFQmNQRkkKbnlWS081UEZMWTBLU3gvT2VjTUxLTGVBUHFPS2dqRzlYTWZjL0o0b21wcVh4YTEvdnkxTFNOTUFtcUdvb2xvLwo2VlFUSlM4cWQ2TVBXemYveDFRckc1MHVsSk9icDRydjU4eFpndGlncjd5ODN0S2llbnprRFF0alVuYz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
                            server: https://172.31.2.120:6443
                        name: kubernetes
                        contexts:
                        - context:
                            cluster: kubernetes
                            user: kubernetes-admin
                        name: kubernetes-admin@kubernetes
                        current-context: kubernetes-admin@kubernetes
                        kind: Config
                        preferences: {}
                        users:
                        - name: kubernetes-admin
                            user:
                            client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM4akNDQWRxZ0F3SUJBZ0lJQ3RmWStxaytldUV3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBek1qQXhOelExTURkYUZ3MHlOVEF6TWpBeE56UTFNVE5hTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXh3Q3NlWmNFL1I4SFQxLzQKMDFBL3dyb29lc3FNeHhBRlZOcVc1L2d5TkxHTWQ1SjBvYlZmbjczNExvUnVWK0FKRzdscDhkNTdVVVRwd2FJNwp5MTN4VGVONm1vTmFYMGh2bmp1QXh3d0lqRElqYU9CK1l1TGlxWGVRWmNWNkpGMGFGM2RXV0VkS1lkdWFLZnluClJpUTJSRjg2Z0VSZU1EenhlUm1zK3FPTi9TcGxIOUpSZ3c5VWNMLzRBaTM5aUpOU1B3MVE0NFh0Tmk1ZFB2YzEKUHE1YndHK25QZm91LzF2TTRwQVgxWnVwNnpzK3lvL1pHM0E5TFhnOUtUY04yQU1aSXZmRVhHSFpOSG0va29hdQp5QkhzWlArV0ZMS0tSR0JrSDA1RVMrT1VJKzhXcjA3d0M5cGNIRm5sQW1EUS9BTVh5aHJoSy9EN0Jvb3ZxWm5NClpWY0tEd0lEQVFBQm95Y3dKVEFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFEemxoY2MvNzFib2JIb3VhdDA5bnBNak5GOGNWcnhLZnZkNgpxTTdRc3ZySlhZQkRIMFZvSVh1RVVqMTRIZlpFaWNuRkdKM1dFNXlSVUVDd09MVXpnSWFrN0NDSjdWanY4OW5ECjJxTEtmc1dxb1JDRVAyR0daTkNGek5VRnZDZWFBWXVTSUlHbHFVdXlxYW9aczFmVFlNNUkyTlkxRmt5MUQ3eXIKQWNTS0lrZzdRVC83ZjVVUHlFUUkwK0xMcTA0NDFWLzJzNkpJSjQ5U3MzVWY1MHF3NVQzb1dscW5VY05xRTNrWgprb3lBQ2E5UHphbW1tQU9rQVNwdXkxL2hDbGwyejZ2RkxWbEZBWmFMU3Q5bzJIWEE5bksyRFR2R0RxNHNhQlh0CmdVdk90dVprRGExRmtnUEpETGJLSnB3Qm1SVGxveEhteUxORWpPUys4LzZvMXU4Wnc3dG9kb0gwdUR3M1ZsK0MKWlFqS2x3Q2lDUGhIeXlTRjcrZUprWGZzR2d6WEFLY0RGSzA4cGZiSmJhOVpLMWZIZmRoYWZ4MFljdUNLU0tlM25sVgp1bTJoaXBYTHM0dC85d1lXSUVmdGR5TjZsS0lJYTVqUVBSZ01hSm5LRjVaVXV1WEZyWk13eTRidFVLSFlvdmNwCkdUR1hJclV5Y1NQSGdzNzBzS1Z2NzhLSmdPbUh1aDFLSXVJeHg2TXhhMVpkSUdCaVhKMW5kS0s0MzBISmNORHEKV2FoKzZPdElIQU9FRXluejRYd2Q4Y3MySGdkK0R6cXF2S3FzZ3h2elhrY3VZbk9oL0JZejlwNkR1TUR3WkRzbgpNS3VtRDNXOFhUaW1uMDYzZjg3Ym04bXdLV2lGVEowRzFmMlV1YWR1Z1NQa25jSVlxQnY1VGd3R0RFbThYZkNtCklhQnBUQ29JY3Y0LzZCUEJtV0NpUGt4MExCS2VKb1NFZ1lFQVJOV3V2TXdQOG9LMmVQTFRhOEZlUHRycFh1VWoKaW9IaFFicWJXOEdiWFFzZz09Ci0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0K
                    """
                    
                    // Remove leading and trailing whitespace from kubeConfig string
                    kubeConfig = kubeConfig.trim()
                    
                    // Set kubeConfig as an environment variable for kubernetesDeploy step
                    env.KUBE_CONFIG_CONTENTS = kubeConfig
                    
                    kubernetesDeploy(
                        kubeconfigContent: env.KUBE_CONFIG_CONTENTS,
                        configs: 'train-schedule-kube-canary.yml',
                        enableConfigSubstitution: true
                    )
                }
            }
        }
        stage('Deploy to Production') {
            environment { 
                CANARY_REPLICAS = 0
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                script {
                    kubernetesDeploy(
                        kubeconfigContent: env.KUBE_CONFIG_CONTENTS,
                        configs: 'train-schedule-kube.yml',
                        enableConfigSubstitution: true
                    )
                }
            }
        }
    }
}
