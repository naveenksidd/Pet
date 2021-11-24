pipeline
{
agent
  {
    label 'Ubuntu'
  }
  environment
  {
    mvnHome=tool 'maven'
  }
stages
{
stage('Code Checkout')
{
steps
{
echo "Code pulling...."
checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/naveenksidd/Pet']]])
}
}
stage('Compile')
{
steps
{
echo "Compile the project"
sh "${mvnHome}/bin/mvn clean compile -f pom.xml"
}
}
stage('Test')
  {
    steps
    {
      echo "Testing..."
      sh "${mvnHome}/bin/mvn test -f pom.xml"
    }
  }
  stage('Integrating with master branch')
  {
    steps
    {
      echo "CI process intiated"
    }
  }
  stage('Build')
  {
    steps
    {
      echo "Building the project......"
       sh "${mvnHome}/bin/mvn clean package -f pom.xml"
    } 
  }
  stage('Artifact storage')
  {
    steps
    {
      echo "artifact pushing to repository"
    }
  }
  statge('Deploying on SIT')
  {
    steps
    {
      echo "artifact deployed on SIT server..."
      deploy adapters: [tomcat9(credentialsId: '28a8ea17-0678-4276-bc4e-56a17f5ad605', path: '', url: 'http://127.0.1.1:8081')], contextPath: 'petclinic.war', war: '**/*.war'
    }
  }
}
}

