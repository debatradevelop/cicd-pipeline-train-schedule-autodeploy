pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = "debatradevelop/train-schedule"
        KUBECONFIG_CONTENTS = '''
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
                client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM4akNDQWRxZ0F3SUJBZ0lJQ3RmWStxaytldUV3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB5TkRBek1qQXhOelExTURkYUZ3MHlOVEF6TWpBeE56UTFNVE5hTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXh3Q3NlWmNFL1I4SFQxLzQKMDFBL3dyb29lc3FNeHhBRlZOcVc1L2d5TkxHTWQ1SjBvYlZmbjczNExvUnVWK0FKRzdscDhkNTdVVVRwd2FJNwp5MTN4VGVONm1vTmFYMGh2bmp1QXh3d0lqRElqYU9CK1l1TGlxWGVRWmNWNkpGMGFGM2RXV0VkS1lkdWFLZnluClJpUTJSRjg2Z0VSZU1EenhlUm1zK3FPTi9TcGxIOUpSZ3c5VWNMLzRBaTM5aUpOU1B3MVE0NFh0Tmk1ZFB2YzEKUHE1YndHK25QZm91LzF2TTRwQVgxWnVwNnpzK3lvL1pHM0E5TFhnOUtUY04yQU1aSXZmRVhHSFpOSG0va29hdQp5QkhzWlArV0ZMS0tSR0JrSDA1RVMrT1VJKzhXcjA3d0M5cGNIRm5sQW1EUS9BTVh5aHJoSy9EN0Jvb3ZxWm5NClpWY0tEd0lEQVFBQm95Y3dKVEFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFEemxoY2MvNzFib2JIb3VhdDA5bnBNak5GOGNWcnhLZnZkNgpxTTdRc3ZySlhZQkRIMFZvSVh1RVVqMTRIZlpFaWNuRkdKM1dFNXlSVUVDd09MVXpnSWFrN0NDSjdWanY4OW5ECjJxTEtmc1dxb1JDRVAyR0daTkNGek5VRnZDZWFBWXVTSUlHbHFVdXlxYW9aczFmVFlNNUkyTlkxRmt5MUQ3eXIKQWNTS0lrZzdRVC83ZjVVUHlFUUkwK0xMcTA0NDFWLzJzNkpJSjQ5U3MzVWY1MHF3NVQzb1dscW5VY05xRTNrWgprb3lBQ2E5UHphbW1tQU9rQVNwdXkxL2hDbGwyejZ2RkxWbEZBWmFMU3Q5bzJIWEE5bksyRFR2R0RxNHNhQlh0CmdVdk90dVprRGExRmtnUEpETGJLSnB3Qm1SVGxveEhteUxORWpPUys4LzZvMXU4Wnc2c2hRVlRrNzFrcnRXRWMKbDlvSHhmeG1kYmxra2hLQndZdGNQbnBWT0VYTXBhMDNxZ0dYN2tXWnpRRk0rN0NCVlZqektySStCRmxJS1JoWQphM2JZaDJZck9wY3BueTMyV1htUmpTRUo2dWhOZmRmdkdpbEd3emRyaEJLcWUwWVpQVGswRE1XeDd2YVl3NUJVCnl3YUkwVk5IZk5JbWhjMXMxSE5qSVBsYk9HRjNlY3FGaDdVVzRudWFvRkVQWnNCV3lvV0d2dG9mcGVQeFpzK3EKa2trdjdtTkt5WWt1VG91bE9EQWd4aFNqRERXbU9RWWZ5cHZwMU9Ca0hEUGdSRnBzZ0VsQmdTMDZ3RFRNVgpsS0UxNWtRYlJwQXlhbXQrSmdjV21qZVhGQnBUNU5sZFBsMkZZNUlvdGZqS0drQkFxdjlpNGJwZ1M5RWNXZjVRCk1ocU1qVWJ3U3VjOXQzTXlVekc2RVdaZ1FMMjVjU2h3RlVjUGt6ZGZsVGUrVjZrTXdnYWtJUFM0MG1ZY0NoZUwKTWkrYzE1YlF5N3J5Y21CT2R6VUZqUHR4aUVoaEV5aTBscldSS0t2LzduSStWWlFBQVF3RXdEQVlEVlIwVEFRSC9CQAp3UUExQmdOVkhSRUVnYjNWdWRISmhkR2x2YmlJNklrNWxkQzV2ZDNkM0xuTmxNUk13RVFZRFZRUUxEQXB2YVc5egpZVzFzTFhOd01JR2ZNQTBHQ1NxR1NJYjNEUUVCQVFVQUE0R05BRENCaVFLQmdRQ3VCQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFEMjJSZ2ZzRjRTK2s0czVTVWw3NjgzUFR5SmQKSjlWTFFpeFFmRnN1bHh0NU9YZ3JzUk9IMXhmODlnM2dSY2tIdmFvSlZQd0VRUHFnUU5nQVZuMllNdEhzOGl6SQpPNWw0S29ONmZWR2Jha2R2RWpib01sNFZsN0NoYVpMRWY5eEdnTUZiT0JtR1JGcmRYejNtL0Z2M2ZpVk4wCkhnSmxkV2J6VVZGWElLemJDRGhENlBlMVlRZ0JyVmRueTlUQ0tOVXlZb3I0RlBBR3dWckRVaU1qZDlrMmJDMm8KR2trRHVpS21KQU1HNnBXbjRYckJEYU54Y1dodVRMUWZpUWdMSmFLRkQ2YVduR3VUMUdzMVdXb1VZcUwzMEhsRApSYTJGY1Y5bkhtR3IzTE1ZczI5WHRaMW12ZDhPY21TaTB1eWpyTkZBTGVkZW9wdnB0R1E5NkpEUTRGUzZvWlFRCnV3eGw3SHc0b3owT1R0R0lmTFdlcmtkc3pQbWgxOHY1ZG5mZ3hqamxZMVUxWVg3bWRVdXRTKzZ6YlNpWmtPRHAKeGoxWWtLTnJUdFNtS1B0WElha3N4TnU3d1VnaG9wa3dkbUlLb2xTd3ZiM3NlRStCY3V2NmZnUzJtWDRiSldMNQpSTXpIcUJCNHc2SjAyblhJUkZnSDdnN3lVQjlBUkJzbFdVYXFLbVdGY0VabDlRZC8wczdrMjI5N1N6RGY0Z1Q1CkFzNzQ0NUNtSjlVQ3Z0RkJqQ2FkWnpFMG9yamZKUytCV3BwZy82Y21jWVozQUNxdzZZZUVoZk5hZGJ1N3V5RmEKa1ZuUDROb3d1T1p1OVpjbjhMYWw4S2xCVmZTd2ZnZ3k2NkJmZ0ZPajB2Z1ZOVTFvMHZ3QzZ4T1RXYklSbWlIeApFT2NZMWxUcXViUkR3TGJxTkFqQ3NRUFQ4eHZMNXRmQ2hUbWRuL2tCclJCYnZSb1ZZNEdldS9hMm9KYTloClNqME50MWhxRzZZd25vMWpZUnVZMkFmTlZHZ3hPSGxxZXFtUGhNZXorZ1hYSzliZjNzVUxCV0FLbWl2ZGdoeXMKSmRpb3lhOTFqT2xXNzZ5ZzEzV1Q2S2lZV3AwOU92RE5sa1g2bkRBNStlMEt1UUxFdU8wZFlBb1g5SHFQTVFtUApGcHd5eWk3dWRsM0g2a3NlVmUyZG1HTmdNS2xKRG9IbVBjN2hsY0lFeFhEV1lpT3Z5ZGt3aUJvRGtYOVcwZG9MCm56cVh1a1lwR0RpQVJpc0t2VjVCSnB0dThRRm5iUStqWWFrTXVtTjZLclpwNnIzOGVYdVZ3VlRQTGRhd1hxT2wKUERXMnNFS0hVSUJSMjlGc3A1OHc0VzhVc0U2bU9sc1ZwUmtBQW9JQkFRREIwZ25NRHVWU0Y2MkV0bjBqZ3UyUgpRQVFoY0VxV0E4cEl3dVN6RGhDTUlBVk8ydXpqNW12clVFN3B1NjdpMFlvT2hWSklpWkV6cGxqTmN0UkYvCk5zRVBKaHRKbGNoYmNpTUxkbTNsVmpRSkJGc0JUcW1aZjNGUWtETzZWTXhPbzFETUNxWmpUU2RHTGR1TlBoTFQKRnN4MTBkWFRoU0tOc09wR1B5VkpRMXNLdmh5bWQzODFvS3o2a1lsdz09Ci0tLS0tRU5EIFBVQkxJQyBLRVkgQkxPQ0stLS0tLQo=
        '''
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
                kubernetesDeploy(
                    kubeconfigContent: env.KUBECONFIG_CONTENTS,
                    configs: 'train-schedule-kube-canary.yml',
                    enableConfigSubstitution: true
                )
            }
        }
        stage('Deploy to Production') {
            environment { 
                CANARY_REPLICAS = 0
            }
            steps {
                input 'Deploy to Production?'
                milestone(1)
                kubernetesDeploy(
                    kubeconfigContent: env.KUBECONFIG_CONTENTS,
                    configs: 'train-schedule-kube.yml',
                    enableConfigSubstitution: true
                )
            }
        }
    }
}
