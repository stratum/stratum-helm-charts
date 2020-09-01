pipeline {
    agent {
        docker { 
            image 'ubuntu:18.04' 
            args '-u root:sudo'
        }
    }
    environment {
        PUBLISH_URL ="charts.stratumproject.org/"
        NEW_REPO_DIR ="stratum-helm-repo"
    }
    stages {
        stage('Install tools') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: "github_repo_deploy_key", keyFileVariable: 'keyfile')]) {

                sh """#!/bin/bash
                set -x
                apt-get update -y
                apt-get install -y curl git rsync
                curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
                helm version
                helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com
                helm repo add rook-release https://charts.rook.io/release
                helm repo add cord https://charts.opencord.org

                mkdir -p ~/.ssh
                ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
                GIT_SSH_COMMAND='ssh -i ${keyfile}' git clone git@github.com:stratum/stratum-helm-repo.git
                git clone "https://gerrit.opencord.org/helm-repo-tools"
                """
              }
            }
        }
        stage('Lint') {
            steps {
                sh """#!/bin/bash
                set -x
                ./helm-repo-tools/helmlint.sh clean
                ./helm-repo-tools/helmrepo.sh
                """
            }
        }
        stage('Update Repo') {
            when {
              branch 'master'
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: "github_repo_deploy_key", keyFileVariable: 'keyfile')]) {
                sh """#!/bin/bash
                set -x
                git config --global user.email "do-not-reply@opennetworking.org"
                git config --global user.name "Jenkins"

                # Tag and push to git the charts repo
                pushd stratum-helm-repo
                
                  # update if charts are changed or the first change
                  set +e
                  if ! git ls-files --others --exclude-standard | grep index.yaml && git diff --exit-code index.yaml > /dev/null; then
                    echo "No changes to charts in patchset"
                    exit 0
                  fi
                  set -e
                
                  # version tag is the current date in RFC3339 format
                  NEW_VERSION=\$(date -u +%Y%m%dT%H%M%SZ)
                
                  # Add changes and create commit
                  git add -A
                  git commit -m "Changed by Stratum Jenkins stratum-helm-chart-publish job: author: $CHANGE_AUTHOR, pr: $GIT_BRANCH"
                
                  # create tag on new commit
                  git tag "\$NEW_VERSION"
                
                  echo "Tags including new tag:"
                  git tag -n
                
                  # push new commit and tag back into repo
                  GIT_SSH_COMMAND='ssh -i ${keyfile}' git push origin
                  GIT_SSH_COMMAND='ssh -i ${keyfile}' git push origin "\$NEW_VERSION"
                popd

                """
                }
            }
        }
        stage('Publish') {
            when {
              branch 'master'
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: "charts.stratumproject.org", keyFileVariable: 'keyfile')]) {
                sh """#!/bin/bash
                set -x
                ssh-keyscan -t rsa charts.stratumproject.org >> ~/.ssh/known_hosts
                rsync -e "ssh -i ${keyfile}"  -rvzh --delete-after --exclude=.git stratum-helm-repo/ github@charts.stratumproject.org:/srv/sites/charts.stratumproject.org/
                """
                }
            }
        }
    }
    post { 
        always { 
            cleanWs()
        }
    }    
}
