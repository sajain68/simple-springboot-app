pipeline {
    agent any
    tools {
        maven 'Maven'
        // nodejs 'NodeJS_14'
        // go 'Go_1_17'
        // ruby 'Ruby_3_0'
        // python 'Python_3_9'
        // dockerTool 'Docker_20_10_8'
        // kubectl 'kubectl_1_21_4'
        // awsCli 'AWS_Cli_2_2_22'
        // terraform 'Terraform_1_0_9'
        // git 'Git_2_33_0'
        // androidSdk 'Android_SDK'
        // xcode 'Xcode_12'
        // ruby 'Ruby_3_0'
        // nodejs 'NodeJS_14'
        // Add More tools as needed
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Detect and Set Java Version') {
            steps {
                script {
                    try {
                        // Function to detect Java version
                        // def javaVersion = detectJavaVersion()

                        // // Set the Java version for the pipeline
                        // env.JAVA_HOME = javaVersion
                        tool name: "Java", type: 'jdk'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Java version detection: ${e.message}")
                    }
                }
            }
        }
        stage('Compile and Run Sonar Analysis') {
            steps {
                script {
                    try {
                        if (fileExists('simple-springboot-app/pom.xml')) {
                            sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=Java-spring-boot -f simple-springboot-app/pom.xml'
                        } else if (fileExists('package.json')) {
                            sh 'npm install'
                            sh 'sonar-scanner' // Run SonarCloud analysis for Node.js application
                        } else if (fileExists('go.mod')) {
                            sh 'go mod download'
                            sh 'sonar-scanner' // Run SonarCloud analysis for Go application
                        } else if (fileExists('Gemfile')) {
                            sh 'bundle install'
                            sh 'sonar-scanner' // Run SonarCloud analysis for Ruby application
                        } else if (fileExists('requirements.txt')) {
                            sh 'pip install -r requirements.txt'
                            sh 'sonar-scanner' // Run SonarCloud analysis for Python application
                        } else {
                            currentBuild.result = 'FAILURE'
                            error("Unsupported application type: No compatible build steps available.")
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Sonar analysis: ${e.message}")
                    }
                }
            }
        }
        stage('RunSCAAnalysisUsingSnyk') {
            steps {
                script {
                    try {
                        withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                            sh 'mvn snyk:test -fn'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Snyk analysis: ${e.message}")
                    }
                }
            }
        }
        stage('Java Spring Boot Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('simple-springboot-app/src/main/resources/application.yml')) {
                            sh 'mvn clean package -f simple-springboot-app/pom.xml'
                            sh 'mvn test -f simple-springboot-app/pom.xml'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Java Spring Boot build and test: ${e.message}")
                    }
                }
            }
        }
        stage('.NET Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('YourSolution.sln')) {
                            sh 'dotnet build'
                            sh 'dotnet test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during .NET build and test: ${e.message}")
                    }
                }
            }
        }
        stage('PHP Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('composer.json')) {
                            sh 'composer install'
                            sh 'phpunit'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during PHP build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Frontend Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('package.json')) {
                            sh 'npm install'
                            sh 'npm test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during frontend build and test: ${e.message}")
                    }
                }
            }
        }
        stage('iOS Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('YourProject.xcodeproj')) {
                            xcodebuild(buildDir: 'build', scheme: 'YourScheme')
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during iOS build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Android Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('build.gradle')) {
                            sh './gradlew build'
                            sh './gradlew test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Android build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Ruby on Rails Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('Gemfile.lock')) {
                            sh 'bundle install'
                            sh 'rake db:migrate'
                            sh 'rails test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Ruby on Rails build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Django Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('manage.py')) {
                            sh 'pip install -r requirements.txt'
                            sh 'python manage.py migrate'
                            sh 'python manage.py test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Django build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Rust Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('Cargo.toml')) {
                            sh 'cargo build'
                            sh 'cargo test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Rust build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Node.js Express Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('app.js')) {
                            sh 'npm install'
                            sh 'npm test' // Assuming the Express app has tests
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Node.js Express build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Ruby Sinatra Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('app.rb')) {
                            sh 'gem install bundler'
                            sh 'bundle install'
                            sh 'ruby -S rake test'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Ruby Sinatra build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Flask Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('app.py')) {
                            sh 'pip install -r requirements.txt'
                            sh 'python -m unittest discover'
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Flask build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Vue.js Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('package.json')) {
                            sh 'npm install'
                            sh 'npm run test' // Assuming the Vue.js app has tests
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Vue.js build and test: ${e.message}")
                    }
                }
            }
        }
        stage('Build and Test') {
            steps {
                script {
                    try {
                        if (fileExists('Dockerfile')) {
                            // Build and push Docker image
                            // docker.build("bwa").push("latest")
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Docker image build: ${e.message}")
                    }
                }
            }
        }
        stage('Kubernetes Deployment') {
            steps {
                script {
                    try {
                        if (fileExists('deployment.yaml')) {
                            // Perform Kubernetes deployment
                            // withKubeConfig([credentialsId: 'kubelogin']) {
                            //     sh ('kubectl apply -f deployment.yaml --namespace=devsecops')
                            // }
                        }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during Kubernetes deployment: ${e.message}")
                    }
                }
            }
        }
        stage('Wait for Testing') {
            steps {
                // Waiting logic here
                sh 'pwd; sleep 180; echo "Application Has been deployed on K8S"'
            }
        }
        stage('Run DAST Using ZAP') {
            steps {
                script {
                    try {
                        // Run ZAP DAST
                        // withKubeConfig([credentialsId: 'kubelogin']) {
                        //     sh('zap.sh -cmd -quickurl http://$(kubectl get services/bwa --namespace=devsecops -o json| jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
                        //     archiveArtifacts artifacts: 'zap_report.html'
                        // }
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("Error during ZAP DAST: ${e.message}")
                    }
                }
            }
        }
    }
}

// Function to detect Java version
def detectJavaVersion() {
    def javaVersionOutput = sh(script: "java -version 2>&1 | head -n 1", returnStatus: true, returnStdout: true).trim()
    def javaVersion = javaVersionOutput.replaceAll('openjdk version "', '').replaceAll('"', '')

    if (javaVersion.startsWith("1.8")) {
        return '8'
    } else if (javaVersion.startsWith("11")) {
        return '11'
    } else if (javaVersion.startsWith("16")) {
        return '16'
    } else {
        error("Unsupported Java version detected: ${javaVersion}")
    }
}
