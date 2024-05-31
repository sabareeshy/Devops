https://spacelift.io/blog/kubernetes-network-policy

https://loft.sh/blog/kubernetes-network-policies-for-isolating-namespaces/


Best practices for Kubernetes Network Policies
Now you’ve seen how to use Network Policies, here are a few best practices that will give you the greatest security protections:

1. Ensure all Pods are covered by a Network Policy
All Pods in a Kubernetes cluster should be subject to Network Policies that limit their network interactions to the minimal set of Ingress/Egress targets they require. Not setting Network Policies allows all Pods to communicate, which is a potential security risk.

2. Use precise Ingress/Egress target selectors
Keep your Pod selectors, namespace selectors, and ipBlock ranges as precise as possible to prevent them from accidentally selecting new Pods in the future. For example, a namespace selector is inappropriate if you’re likely to deploy additional Pods to the namespace, and those Pods shouldn’t automatically communicate with your Network Policy’s target.

3. Set a default deny policy, then add your allow policies
You can ensure your cluster has complete Network Policy coverage by creating a default “deny all” policy (as shown in the example above), then adding specific “allow” policies that authorize required traffic flows. This method means new Pods are protected from accidental network exposure, even if you forget to create a specific Network Policy for them.

4. Regularly review your policies and keep them updated
Your Network Policy requirements are likely to change as your cluster evolves with new Pods and namespaces. You should regularly review your policies and make any alterations required so they remain appropriate for your environment.

5. Test your policies to check they’re working as intended
One of the difficulties associated with Network Policies is the lack of clear visibility into whether they’re working. It’s worthwhile to test new policies to be sure they’re configured correctly. As in the examples above, you can create a new Pod with labels that match your Network Policy’s selectors, then use commands like curl and ping to test the connectivity available within the container.

Finally, you should ensure that Network Policies are only part of your Kubernetes isolation strategy. They need to be supported by other protections too, such as correct container security contexts and appropriate RBAC rules that block unauthorized user access to your Pods.


Securing Control Plane Components:

Now let’s talk about securing etcd, where all your cluster configuration and desired state persist. If attackers get access to the etcd datastore, they can control your cluster and run containers with elevated privileges on any cluster node. You can secure it by using Public Key Infrastructure, which uses a combination of keys and certificates. In addition, this ensures that data in transit is secure using TLS and access is restricted using credentials.

The Kubernetes API server is another essential component that allows external clients to communicate, know, and update the state of your cluster. Therefore, it is vital to make sure it’s secure from external attacks.

The API server typically listens on secure TLS ports such as 443, 8843, and so on. You can get details about the API server endpoint using the kubectl cluster-info command. If you try to access the API server endpoint, you will most probably get the following response, which means that it is inaccessible from the internet. It also means that you are not authorized to access the API server endpoint.

Copy
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {

  },
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {

  },
  "code": 403
}
Similar to etcd, the Kubernetes API server can also be secured using the PKI and TLS.

#

