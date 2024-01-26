
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
        stage('Backend') {
            parallel {
                stage('Destroyig Catalogue') {
                    steps {
                        dir('catalogue') {   
                            git branch: 'main', url: 'https://github.com/Shoaibs411/catalogue.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.2 -auto-approve
                                ''' 
                            }
                        }
                    } 
                stage('Destroyig User') {
                    steps {
                        dir('user') {  git branch: 'main', url: 'https://github.com/Shoaibs411/user.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Destroyig Cart') {
                    steps {
                        dir('cart') { git branch: 'main', url: 'https://github.com/Shoaibs411/cart.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.2 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Destroyig Shipping') {
                    steps {
                        dir('shipping') { git branch: 'main', url: 'https://github.com/Shoaibs411/shipping.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
                                ''' 
                            }
                        }
                    }
                stage('Destroyig Payment') {
                    steps {
                        dir('payment') { git branch: 'main', url: 'https://github.com/Shoaibs411/payment.git'
                                sh ''' 
                                    cd mutable-infra
                                    terrafile -f env-${ENV}/Terrafile
                                    terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                    terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
                                ''' 
                                }
                            }
                        }
                    }
                }
        stage('Destroyig Frontend') {
                steps {
                        dir('frontend') {  git branch: 'main', url: 'https://github.com/Shoaibs411/frontend.git'
                            sh ''' 
                                cd mutable-infra
                                terrafile -f env-${ENV}/Terrafile
                                terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -var APP_VERSION=0.0.1 -auto-approve
                            ''' 
                        }
                    }
                }

        stage('Destroying Loadbalancers') {
            steps {
                dir('ALB') {
                git branch: 'main', url: 'https://github.com/Shoaibs411/terraform-loadbalancers.git'
                        sh '''
                            sleep 15
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                            '''
                }
            }
        }

        stage('Destroying Databases') {
            steps {
                dir('DB') {
                git branch: 'main', url: 'https://github.com/Shoaibs411/terraform-databases.git'
                        sh '''
                            rm -rf .terraform
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                        '''
                }
            }
        }
        stage('Destroying VPC') {
            steps {
                dir('VPC') {
                git branch: 'main', url: 'https://github.com/Shoaibs411/terraform-vpc.git'
                        sh '''
                            terrafile -f env-dev/Terrafile
                            terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform destroy -auto-approve -var-file=env-${ENV}/${ENV}.tfvars -var ENV=${ENV}
                        '''
                }
            }
        }
    }
}