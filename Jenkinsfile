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
                        def mvnHome = tool name: 'maven3', type: 'maven'
                        sh "${mvnHome}/bin/mvn -Drevision=${MY_BUILD_VERSION} clean package"

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
					
                                        def mvnHome = tool name: 'maven3', type: 'maven'
					sh "${mvnHome}/bin/mvn -Drevision=${MY_BUILD_VERSION} clean deploy"
					
				}
			}

		}

    }  

}
