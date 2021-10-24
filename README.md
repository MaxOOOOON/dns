# dns


Домашнее задание

    настраиваем split-dns

    взять стенд https://github.com/erlong15/vagrant-bind  - vagrant-bind-master

    добавить еще один сервер client2     

    завести в зоне dns.lab 
    имена
    web1 - смотрит на клиент1
    web2  смотрит на клиент2

    завести еще одну зону newdns.lab  
    завести в ней запись
    www - смотрит на обоих клиентов

    настроить split-dns  
    клиент1 - видит обе зоны, но в зоне dns.lab только web1  
    клиент2 видит только dns.lab    

    настроить все без выключения selinux





Запуск 

    Vagrant up

Затем 

    ansible-playbook vagrant-bind-master/provisioning/playbook.yml


## Завести в зоне dns.lab   
Добавлены записи в named.dns.lab

    web1            IN      A       192.168.50.151
    web2            IN      A       192.168.50.161


 ## Завести еще одну зону newdns.lab  
Создана зона named.newdns.lab 

        $TTL 3600
    $ORIGIN newdns.lab.
    @               IN      SOA     ns01.dns.lab. root.dns.lab. (
                                2711201407 ; serial
                                3600       ; refresh (1 hour)
                                600        ; retry (10 minutes)
                                86400      ; expire (1 day)
                                600        ; minimum (10 minutes)
                            )

                    IN      NS      ns01.dns.lab.
                    IN      NS      ns02.dns.lab.

    ; DNS Servers
    www            IN      A       192.168.50.15
    www            IN      A       192.168.50.16

и прописана в named.conf

    zone "newdns.lab" {
        type master;
        allow-transfer { key "zonetransfer.key"; };
        file "/etc/named/named.newdns.lab";
    };

## Split-dns
Для двух клиентов созданы 2 view: client1 и client2  
Также прописаны acl для 2х клиентов


    acl "net1" { 192.168.50.15; };
    acl "net2" { 192.168.50.16; };



Проверка:


1й клиент
![]()  


2й клиент
![]()  