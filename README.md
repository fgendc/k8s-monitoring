## Iniciar Vagrant
Editar la variable cluster para añadir o cambiar los nodos
```
cluster = {
  "master" => { :ip => "192.168.99.200", :cpus => 2, :mem => 4096 },
  "slave" => { :ip => "192.168.99.201", :cpus => 4, :mem => 4096 }
}
```

## Instalar kubernetes
En nodo con ansible instalar python3, pip3 y los requirements de kubespray (probado en WSL con Ubuntu):
```
sudo apt update && sudo apt install python3-pip
sudo pip3 install -r requirements.txt

```

Configurar e instalar k8s con kubespray. Asegurarse de que el nodo de ansible se puede conectar a los nodos.

```

declare -a IPS=(192.168.99.200 192.168.99.201)
CONFIG_FILE=inventory/mycluster/hosts.yml python3 contrib/inventory_builder/inventory.py ${IPS[@]}

ansible-playbook -i inventory/mycluster/hosts.yml cluster.yml -b -v   --private-key=~/.ssh/id_rsa -u vagrant --become
```