pipeline {
  agent {
    node {
      label 'builder'
      customWorkspace '/mnt/los-build/lineage-16.0'
    }
  }
    environment {
        MIRROR_PATH             = '/mnt/los-mirror/LineageOS/android.git'
        BUILD_PATH              = '/mnt/los-build/lineage-16.0'
        
	BRANCH                  = 'lineage-16.0'
        DEVICE                  = 'jfltexx'
        
	USE_CCACHE              =  '1'
        CCACHE_COMPRESS         =  '1'
        ANDROID_JACK_VM_ARGS    =  '-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4G'
    }
    stages {
        stage('Preparation') {
            steps {
                echo 'Preparation'
		//echo "Will deploy to ${DEPLOY_ENV}"
                //sh 'mkdir -p ${BUILD_PATH}'
                dir("${BUILD_PATH}") {
                    sh("pwd")
                    sh 'mkdir -p ~/bin'
                    sh 'curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo'
                    sh '''#!/bin/bash\nset -x\nsource ~/.profile\nrepo init -u ${MIRROR_PATH} -b ${BRANCH}'''
                }
            }
        }
        stage('Code syncing') {
            steps {
                echo 'Code syncing'
                dir("${BUILD_PATH}") {
		    sh '''rm -rf .repo/local_manifests/*'''
		    sh '''wget https://raw.githubusercontent.com/los-legacy/local_manifests/lineage-16.0/jfltexx.xml -O .repo/local_manifests/jfltexx.xml'''
                    sh '''#!/bin/bash\nset +x\nsource ~/.profile\nrepo sync -f --force-sync --force-broken --no-clone-bundle --no-tags -j$(nproc --all)'''
                }
            }
        }
        stage('Build process') {
            steps {
                echo 'Build process'
                dir("${BUILD_PATH}") {
                    sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"\nbrunch "${DEVICE}"'''
                }    
            }
        }
        stage('OTA Package') {
            steps {
                echo 'OTA Package'
                dir("${BUILD_PATH}") {
                    //sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"'''
                }    
            }
        }
        stage('Archiving') {
            steps {
                echo 'Archiving'
                dir("${BUILD_PATH}") {
                    //sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"'''
                }    
            }
        } 
        stage('Publishing') {
            steps {
                echo 'Publishing'
                dir("${BUILD_PATH}") {
                    //sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"'''
                }    
            }
        }         
    }
}
