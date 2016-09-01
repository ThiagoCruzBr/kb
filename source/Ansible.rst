Ansible
=======

Instalação
---------

# yum install epel-release wget
# yum install ansible

Inventarios
-----------
O ansible conecta nas máquinas de acordo com os endereços registrados no seu arquivo "hosts".
Nesta configuraçáo é possível definir hosts individuais grupos...vide documentacao

    #vi /etc/ansible/hosts

AD-HOC
------
Uma das formas para executar um comando remotamente pelo Ansible é adicionar nos hosts conhecidos do ssh (known_hosts) da seguinte forma:
* Apenas execute o ssh para host de destino para que ele seja adicionado e diga "yes"

    # ssh 192.168.25.14
    ....
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '192.168.25.14' (ECDSA) to the list of known hosts.
    root@192.168.25.14's password:

Agora é possível executar alguns comandos remotamente. Exemplo:
# ansible servidores -k -a "whoami"
SSH password:
192.168.25.14 | SUCCESS | rc=0 >>
root

192.168.25.15 | SUCCESS | rc=0 >>
root
