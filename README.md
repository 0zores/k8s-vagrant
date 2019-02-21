# k8s-vagrant

Deploy a multi-node cluster for Kubernetes using Vagrant + Ansible

## What do you need

- Vagrant >= 2.1.2
- Ansible >= 2.7

## How to use

- Install the requirements
- Clone this repo
- Run `vagrant up` to spin up the cluster
- 4 machines should be created: 
    - Master:
        - k8smaster
    - Nodes:
        - k8snode01
        - k8snode02
        - k8snode03

## Validating

- Access the master machine using `vagrant ssh k8smaster`
- Run the command `kubectl get nodes`
    - To see the status of all kubernetes nodes, you should see 4 nodes with Ready status
- Run the command `kubectl get pods -n kube-system`
    - To see the status of all pods of the kube-system namespace, you should see all the pods with Running status
    - Except one `calico-xxxx` pod that sometimes remains in the Error state, but for now this is not a big issue

## Important Notices

- I tested heavily under Fedora Linux and using libvirt provider, but I think this should work using a different OS and a different provider (at least VirtualBox and VMWare should work)
- For now, I'm using the kubeadm method to deploy the cluster
    - Deploy using the [hard way](https://github.com/kelseyhightower/kubernetes-the-hard-way) method is on the way
- kubectl is usable only from k8smaster machine
    - if you need, just run `vagrant ssh k8smaster` to access the machine
- I'm using the [Project Calico](https://www.projectcalico.org/) pod network addon
- Kubernetes Dashboard should be installed by default as well
- There is a lot of things to do yet, but this is my MVP