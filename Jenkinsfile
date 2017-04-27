pipeline {
    agent any
    tools {
        nodejs 'node-v4.4.4'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Bundle') {
            steps {
                sh """
                    find . -name ".npmignore" -o -name ".gitignore" -delete
                    sed -e 's/{{NODE_VERSION}}/4.4.4/g' debian/limitd_defaults.template > debian/limitd_defaults
                    
                    fpm --deb-user limitd --deb-group limitd \
                    --before-install debian/pre_install.sh --after-install debian/post_install.sh \
                    --before-remove debian/pre_rm.sh \
                    --prefix /opt/auth0 --deb-upstart debian/limitd --deb-default debian/limitd_defaults \
                    --version ${env.BUILD_ID} -n limitd \
                    -d auth0-node-v4.4.4-linux-x64 \
                    -x '**/.git*' -x '*.tgz' -x '**/test/*' \
                    --description 'Jenkins build ${env.BUILD_ID}' \
                    -t deb -s dir .=/limitd 
				"""
                sh "mv limitd_${env.BUILD_ID}_amd64.deb ${params.bundle_file}"
                archiveArtifacts "${params.bundle_file}"
            }
        }
    }
}