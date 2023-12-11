helm install jenkins -f helm/jenkins-values.yaml stable/jenkins --namespace kyverno-demo --version 0.22.0
printf $(kubectl get secret --namespace kyverno-demo jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
export NODE_PORT=$(kubectl get --namespace kyverno-demo -o jsonpath="{.spec.ports[0].nodePort}" services jenkins)
export NODE_IP=$(kubectl get nodes --namespace kyverno-demo -o jsonpath="{.items[0].status.addresses[0].address}")
echo http://$NODE_IP:$NODE_PORT/login


kubectl cluster-info
Add a new cloud and copy the config file from $HOME/.kube/config (replace the file references with base64 encoded version of the file contents
Run the below script in script console if you get 403 while configuring clouds (kubernetes)
import jenkins.model.Jenkins

def instance = Jenkins.instance
instance.setCrumbIssuer(null)

kubectl patch configmap/ci-gates -p '{"data":{"secrets_scan":"fail"}}' -n kyverno-demo
