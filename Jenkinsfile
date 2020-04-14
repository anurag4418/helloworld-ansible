pipeline {
	agent any

    environment
    {
        GIT_BRANCH_NAME = "1"
        MY_BUILD_VERSION = "1"
    }

    stages {
       
       stage('Scm-Checkout') {
       	  steps {
       	  	script {

       	  		my_scm_fn = checkout changelog: false, scm: [$class: 'GitSCM', branches: [[name: '**']], 
       	  		doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
       	  		userRemoteConfigs: [[credentialsId: 'git_creds', url: 'https://github.com/anurag4418/helloworld-ansible.git']]]
       	  		echo "${my_scm_fn}"
       	  		MY_BUILD_VERSION = my_scm_fn.GIT_COMMIT[0..4]
       	  		echo MY_BUILD_VERSION
       	  		GIT_BRANCH_NAME = my_scm_fn.GIT_BRANCH

       	  		sh "mvn -Drevision=${MY_BUILD_VERSION} clean install"

       	  	}
       	  }
       }

		stage("Publish")
		{
			when
			 {
				    expression { 
				        GIT_BRANCH_NAME ==~ /.*master|.*feature|.*develop|.*hotfix/
				    }
			}
			steps
			{
				script
				{
					
					sh "mvn -Drevision=${MY_BUILD_VERSION} clean deploy"
					
				}
			}

		}

    }  

}
