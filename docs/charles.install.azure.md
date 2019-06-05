git clone git@github.com:openshift/installer.git $GOPATH/src/github.com/openshift/installer
cd installer
hack/build.sh
export OPENSHIFT_INSTALL_OS_IMAGE_OVERRIDE="/resourceGroups/rhcosimages/providers/Microsoft.Compute/images/rhcos-410.8.20190504.0-azure.vhd"
bin/openshift-install create install-config --log-level=debug --dir $USERNAME-os4
# answer questions..

bin/openshift-install create cluster --dir $USERNAME-os4  --log-level=debug 
# wait a long time: mine timed out, but the cluster was functional
# fix CSRs
export KUBECONFIG=$USERNAME-os4/auth/kubeconfig
oc get csr --no-headers | grep Pending | awk '{print $1}' | xargs --no-run-if-empty oc adm certificate approve
