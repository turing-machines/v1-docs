# Reset K3s

If you mess anything up in your Kubernetes cluster, and want to start from scratch, you can use Ansible's playbook`reset`

```text
ansible-playbook reset.yml -i inventory/hosts.ini
```

### Shutting down the Turing Pi Cluster

Instead of just unplugging the Turing Pi, it's best to safely shut down all the Raspberry Pis. Instead of logging into each Pi and running the `shutdown` command, you can use Ansible, since it already knows how to connect to all the Pis. Just run this command:

```text
ansible all -i inventory/hosts.ini -a "shutdown now" -b
```

Ansible will report failure for each server since the server shuts down and Ansible loses the connection, but you should see all the Pis power off after a minute or so. Now it's safe to unplug the Turing Pi.



_This guide is based on the article_ [_Installing K3s Kubernetes on the Turing Pi_](https://www.jeffgeerling.com/blog/2020/installing-k3s-kubernetes-on-turing-pi-raspberry-pi-cluster-episode-3) _by Jeff Geerling_

