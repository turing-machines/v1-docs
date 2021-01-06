# Installing K3s

We will install K3s using ansible. 

Download or clone the `k3s-ansible` repository, modify the playbook inventory, and run it:

1. Download using the 'Download ZIP' link on GitHub on [https://github.com/rancher/k3s-ansible](https://github.com/rancher/k3s-ansible)
2. Edit the Ansible inventory file `inventory/hosts.ini`, and replace the examples with the IPs or [hostnames](https://docs.turingpi.com/install-kubernetes-k3s/change-hostnames) of your master and nodes. This file describes the K3s masters and nodes to Ansible as it installs K3s.
3. Edit the `inventory/group_vars/all.yml` file and change the `ansible_user` to `pirate`.
4. Run `ansible-playbook site.yml -i inventory/hosts.ini` and wait.

To connect to the cluster, once it's built, you need to grab the `kubectl` configuration from the master:

```text
scp pirate@turing-master:~/.kube/config ~/.kube/config-turing-pi
```

Make sure you have `kubectl` installed on your computer \(you can install it following [these directions](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\).

Then set the `KUBECONFIG` environment variable, and start running `kubectl` commands:

```text
export KUBECONFIG=~/.kube/config-turing-pi
kubectl get nodes
```

You should get a list of all the Pi servers; if you do, congratulations! Your cluster is up and running.



_This guide is based on the article_ [_Installing K3s Kubernetes on the Turing Pi_](https://www.jeffgeerling.com/blog/2020/installing-k3s-kubernetes-on-turing-pi-raspberry-pi-cluster-episode-3) _by Jeff Geerling_

