properties([pipelineTriggers([githubPush()])])
node('linux')
{
	stage('Unit Tests')
	{
		sh 'ant -f test.xml -v'
		junit 'reports/*.xml'
	} 
	stage('Build')
	{
		sh 'ant -f build.xml -v'
	} 
	stage('Deploy')
	{
		sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://nainafeng-seis-assign10/'
	}
	stage('Report')
	{
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'd4a0841c-ba93-4d05-b60b-02a82e6f066c', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) 
        {
			sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
        }
	}
}
