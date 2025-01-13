# `sou-lab-cni`
Questa repository contiene il progetto "sou-lab-cni" che ha l'obiettivo di creare un ambiente virtuale multinodo utilizzando Vagrant, Ansible, Podman e HAProxy. 

L'architettura prevede due nodi:

- **soufe1**: Nodo frontend che funge da reverse proxy utilizzando HAProxy.
```vagrantfile
config.vm.define "soufe1" do |soufe1|
  soufe1.vm.box = "rockylinux/9"
  soufe1.vm.network "private_network", ip: "192.168.10.10"
  soufe1.vm.hostname = "soufe1"
  soufe1.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "./inventory.yml"
```
- **soube2**: Nodo backend che ospita i servizi Prometheus e Grafana in container gestiti da Podman.
```vagrantfile
config.vm.define "soube2" do |soube2|
  soube2.vm.box = "rockylinux/9"
  soube2.vm.network "private_network", ip: "192.168.10.20"
  soube2.vm.hostname = "soube2"
  soube2.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.inventory_path = "./inventory.yml"
```

L'infrastruttura è configurata per permettere l'accesso a Grafana e Prometheus tramite HAProxy, con URL dedicati.

---
## Struttura della repository
```
sou-lab-cni
└── modulo_progetto
    ├── Vagrantfile
    ├── inventory.yml
    ├── playbook.yml
    └── roles
        └── sou_podman
            ├── tasks
            │   └── main.yml
            └── templates
                ├── grafana.ini.j2
                ├── haproxy.cfg.j2
                └── prometheus.yml.j2

6 directories, 7 files
```
---
## Configurazione della Rete

I nodi sono configurati per comunicare su una rete privata. Gli indirizzi IP assegnati sono:

- **soufe1**: `192.168.10.10`
- **soube2**: `192.168.10.20`
---
## passaggi aggiuntivi 
Per poter far si che HAProxy svolga il compito di reverse proxy e permetta l'accesso a Prometheus e Grafana devi modificare il file /etc/hosts per creare un DNS locale e quindi un'associazione tra ip del nodo ospitante Prometheus e Grafana con il nome di dominio che preferisci per i due Server.

 ---
 ## Requisiti
 Assicurarati di avere installati i seguenti strumenti sul tuo sistema:

- [Vagrant](https://www.vagrantup.com/)
- [Ansible](https://www.ansible.com/)
- [Podman](https://podman.io/)
- [VirtualBox](https://www.virtualbox.org/) o un altro provider di virtualizzazione compatibile con Vagrant
 
