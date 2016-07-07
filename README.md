# vagrant-multihost-sample-ansible

Vagrant provisioning multihost instances:

* Uses Ansible as provisioner.
* Implement Roles (Groups) Pattern to provision multi-instances with different Profiles to be provioned.
* Creates VMs with custom parameters as Ram, HD size, CPUs, etc.



## 1. Clone the repository

```bash
$ git clone https://github.com/chilcano/vagrant-multihost-sample-ansible

Cloning into 'vagrant-multihost-sample-ansible'...
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
Checking connectivity... done.
```

```bash
$ tree vagrant-multihost-sample-ansible

vagrant-multihost-sample-ansible
├── README.md
├── Vagrantfile
├── playbook.yml
├── roles
│   ├── all_role
│   │   └── tasks
│   │       └── main.yml
│   ├── hadoop_role
│   │   └── tasks
│   │       └── main.yml
│   ├── kafka_role
│   │   └── tasks
│   │       └── main.yml
│   ├── postgresql_role
│   │   ├── files
│   │   │   └── pg_hba.conf
│   │   ├── handlers
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── vars
│   │       └── main.yml
│   └── zookeeper_role
│       └── tasks
│           └── main.yml
└── shared_dir_host

15 directories, 11 files
```



## 2. Run Vagrant

```bash
$ cd vagrant-multihost-sample-ansible

$ vagrant up

 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-1/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-2/box-disk1.vdi) doesn't exist.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-2/box-disk1.vdi) doesn't exist.
Bringing machine 'box-zookeeper-1' up with 'virtualbox' provider...
Bringing machine 'box-zookeeper-2' up with 'virtualbox' provider...
...
TASK [Simple CMD for "PostgreSQL hosts"] ***************************************
changed: [box-hadoop-1]
changed: [box-hadoop-2]

RUNNING HANDLER [postgresql_role : Restart Postgres] ***************************
changed: [box-hadoop-1]
changed: [box-hadoop-2]

PLAY RECAP *********************************************************************
box-hadoop-1               : ok=16   changed=12   unreachable=0    failed=0
box-hadoop-2               : ok=16   changed=12   unreachable=0    failed=0
box-kafka-1                : ok=8    changed=5    unreachable=0    failed=0
box-kafka-2                : ok=8    changed=5    unreachable=0    failed=0
box-zookeeper-1            : ok=8    changed=5    unreachable=0    failed=0
box-zookeeper-2            : ok=8    changed=5    unreachable=0    failed=0
```



## 3. Check provisioned VMs

```bash
$ vagrant status

 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-1/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-1/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-2/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-zookeeper-2/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-1/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-1/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-2/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-kafka-2/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-1/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-1/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-2/box-disk1.vdi) has been customized.
 --> VM/VDI file (/Users/Chilcano/VirtualBox VMs/box-hadoop-2/box-disk1.vdi) has been customized.
Current machine states:

box-zookeeper-1           running (virtualbox)
box-zookeeper-2           running (virtualbox)
box-kafka-1               running (virtualbox)
box-kafka-2               running (virtualbox)
box-hadoop-1              running (virtualbox)
box-hadoop-2              running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

```bash
$ vagrant ssh box-zookeeper-1

$ vagrant destroy

```
