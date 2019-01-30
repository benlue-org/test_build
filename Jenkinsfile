pipeline {
  agent {
    node {
      label 'master'
      customWorkspace '/mnt/los-build'
    }
  }
    environment {
        MIRROR_PATH             = '/mnt/los-mirror/LineageOS/android.git'
        BUILD_PATH              = '/mnt/los-build'
        DEVICE_PATH             = '/mnt/los-build/device'        
        
	BRANCH                  = 'lineage-15.1'
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
                    sh 'repo init -u ${MIRROR_PATH} -b "${BRANCH}"'
                }
            }
        }
        stage('Code syncing') {
            steps {
                echo 'Code syncing'
                dir("${BUILD_PATH}") {
                    sh '''#!/bin/bash\nset +x\nrepo sync -f --force-sync --force-broken --no-clone-bundle --no-tags -j$(nproc --all)'''
                }
            }
        }
        stage('Jfltexx Source') {
            steps {
                echo 'jfltexx Source'
                dir("${DEVICE_PATH}") {
                    checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					extensions: [[$class: 'RelativeTargetDirectory', 
                        relativeTargetDir: 'qcom/common']],
					userRemoteConfigs: [[url: 'https://github.com/LineageOS/android_device_qcom_common.git']]
					])
					/*
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					userRemoteConfigs: [[url: 'https://github.com/LineageOS/android_device_samsung_qcom-common.git']]
					])
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					userRemoteConfigs: [[url: 'https://github.com/LineageOS/android_hardware_samsung.git']]
					])
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					userRemoteConfigs: [[url: 'https://github.com/LineageOS/android_packages_resources_devicesettings.git']]
					])
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					userRemoteConfigs: [[url: 'https://github.com/los-legacy/android_device_samsung_jf-common.git']]
					])
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					userRemoteConfigs: [[url: 'https://github.com/los-legacy/android_device_samsung_jfltexx.git']]
					])
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					extensions: [[$class: 'CloneOption', timeout: 120]],
					userRemoteConfigs: [[url: 'https://github.com/los-legacy/android_kernel_samsung_jf.git']]
					])
					checkout([$class: 'GitSCM',
					branches: [[name: 'origin/lineage-15.1']],
					extensions: [[$class: 'CloneOption', timeout: 120]],
					userRemoteConfigs: [[url: 'https://github.com/los-legacy/proprietary_vendor_samsung_jf.git']]
					])
					*/
                }    
            }
        }
        stage('Build process') {
            steps {
                echo 'Build process'
                dir("${BUILD_PATH}") {
                    sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"\nprintenv'''
                }    
            }
        }
        stage('OTA Package') {
            steps {
                echo 'OTA Package'
                dir("${BUILD_PATH}") {
                    //sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"\nprintenv'''
                }    
            }
        }
        stage('Archiving') {
            steps {
                echo 'Archiving'
                dir("${BUILD_PATH}") {
                    //sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"\nprintenv'''
                }    
            }
        } 
        stage('Publishing') {
            steps {
                echo 'Publishing'
                dir("${BUILD_PATH}") {
                    //sh '''#!/bin/bash\nsource build/envsetup.sh\nbreakfast "${DEVICE}"\nprintenv'''
                }    
            }
        }         
    }
}
