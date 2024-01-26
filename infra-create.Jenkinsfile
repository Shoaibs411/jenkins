pipeline {
     agent {
        label 'ws'
    } 
    options {
        ansiColor('xterm')
    }
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select the environment')
    }
    stages {
        stage('Creating VPC') {
            steps {
                dir('VPC') {
                git branch: 'main', url: 'https://github.com/Shoaibs411/terraform-vpc.git'
                        sh '''
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                        '''
                }
            }
        }

        stage('Creating Loadbalancers') {
            steps {
                dir('ALB') {
                git branch: 'main', url: 'https://github.com/Shoaibs411/terraform-loadbalancers.git'
                        sh '''
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            '''
                }
            }
        }

        stage('Creating Databases') {
            steps {
                dir('DB') {
                git branch: 'main', url: 'https://github.com/Shoaibs411/terraform-databases.git'
                        sh '''
                            rm -rf .terraform
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                        '''
                }
            }
        }

        stage('Backend') {
            parallel {
                stage('Creating Catalogue') {
                    steps {
                        dir('catalogue') {   
                            git branch: 'main', url: 'https://github.com/Shoaibs411/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.2
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.2 -auto-approve
                                ''' 
                            }
                        }
                    } 
                stage('Creating User') {
                    steps {
                        dir('user') {  
                            git branch: 'main', url: 'https://github.com/Shoaibs411/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1
                                    terraform apply -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1  -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Creating Cart') {
                    steps {
                        dir('cart') { 
                            git branch: 'main', url: 'https://github.com/Shoaibs411/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.2
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.2 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Creating Shipping') {
                    steps {
                        dir('shipping') { 
                            git branch: 'main', url: 'https://github.com/Shoaibs411/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Creating Payment') {
                    steps {
                        dir('payment') { 
                            git branch: 'main', url: 'https://github.com/Shoaibs411/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    rm -rf .terraform
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1
                                    terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
                                ''' 
                                }
                            }
                        }
                    }
                }
        // stage('Creating Frontend') {
        //         steps {
        //                 dir('frontend') {  git branch: 'main', url: 'https://github.com/Shoaibs411/frontend.git'
        //                     sh ''' 
        //                         cd mutable-infra
        //                         rm -rf .terraform
        //                         terrafile -f env-${ENV}/Terrafile
        //                         terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
        //                         terraform plan -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1
        //                         terraform apply -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
        //                     ''' 
        //                 }
        //             }
        //         }
            }
        post {
                always {
                    cleanWs()
                }
            }
    }