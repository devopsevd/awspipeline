node{

    checkout scm
    def mvnHome
    dir('BuildQuality'){
        stage('Preparation'){
                        
            git 'https://github.com/devopsevd/simple-spring.git'
            mvnHome = tool 'Maven'
        }

        stage('Build') {
            // Run the maven build
            if (isUnix()) {
                sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
            } else {
                bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
            }
        }
        
        stage('SonarQube Analysis') { 
           // def mvnHome
            //mvnHome = tool 'Maven'
            withSonarQubeEnv('Sonar') { 
                if (isUnix()) {
                    sh "'${mvnHome}/bin/mvn' org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f pom.xml "+ 
                    " -Dsonar.projectKey=org.sonarqube:java-sonar " +
                    " -Dsonar.projectKey=org.sonarqube:java-sonar " +
                    " -Dsonar.projectName='Java :: Simple Spring Project' " +
                    " -Dsonar.projectVersion=1.0 " +
                    " -Dsonar.language=java " +
                    " -Dsonar.sources=. " +
                    " -Dsonar.tests=. " +
                    " -Dsonar.test.inclusions='**/*Test*/**' " +
                    " -Dsonar.exclusions='**/*Test*/**' "
                } else {
                    bat (/"${mvnHome}\bin\mvn" org.sonarsource.scanner.maven:sonar-maven-plugin:3.3.0.603:sonar -f pom.xml -Dsonar.projectKey=org.sonarqube:java-sonar -Dsonar.projectName="Java :: Simple Spring Project" /)
                }    
            }        
        }

        stage('Unit Test Results') {
            junit '**/target/surefire-reports/TEST-*.xml'
            //archive 'target/*.jar'
        }

    }
}
