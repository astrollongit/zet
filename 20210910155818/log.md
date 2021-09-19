## Friday, September 17, 2021, 12:30:05PM EDT

Q: Do the control plane resource require a Kubernetes Node on can they
be just any machine?

All control plane components must be run on a Kubernetes Node (a machine
with `kublet`, etc. installed).

Q: What are the ports for `kube-apiserver`?

* 8080 for http
* 6443 for https

Q: How does `kube-apiserver` route requests?

`kubeadm` deploys every controller as a pod in `kube-system` namespace
and the `kube-apiserver` simply routes requests to *registered*
controllers/providers/pods. The `kube-apiserver` seems to be responsible
for registering everything with which it can broker communication.
It waits for an answer from the thing providing the response and then
sends it. It seems to handle time-outs as well and can be scaled
laterally by adding more instances of `kube-apiserver`.￼

> FunFact: You can interrupt (Control-C) `k delete ...` (and other long
> running stuff) rather than waiting around because the `kube-apiserver`
> has already initiated the work and all you are waiting for is the
> response from the controller.

Q: What do I have to do to make a 'Node'?

* Install container runtime (`docker`, `cri-o`, etc.)
* Install `kubelet`
* Install `kubeproxy`

Q: What should I do to practice this?

1. Install `minikube` locally and play with it
1. Build a virtual cluster using only VirtualBox (VMs)
1. Build a small on-prem hardware cluster
1. Build a small cloud cluster
1. Build a small hybrid cluster

Q: Can `kube-proxy` *write* to iptables config?

It definitely does attach rules to the "NAT pre-routing" hook in
iptables.

Q: Are all control plane resources in the same namespace?

By default, yes `kube-system` which you can see with `k get po -A`

Q: When Kubernetes control plane is installed, Core DNS is also?

Yes, by default (I think).