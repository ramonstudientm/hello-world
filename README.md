# Minha rede de servidor linux

se for utilizar de modo virtualizado o servidor firewall/dhcp/dns/ad, precisamos atentar e ter uma placa de rede em bridge e outra em rede interna

após instalação, verificar layout do teclado
$ dpkg-reconfigure keyboard-configuration > other > selecione linguagem e pais > ok, 
 
 
edite a lista de pacotes do gerenciador de pacotes, é bom adicionar as opções non-free e contrib para caso fizer uma instalação de algum pacote que não seja fornecido pelos oficiais da distro.

$ deb  url main contrib non-free

edite  alinha do pacote de seguranças e coloque o oficial da distro caso esteja outro.


edite o resolv.conf caso queira adicionar um dns publico de sua preferencia

edite o arquivo das interfaces de rede

# interfaces de rede
#essa linha abaixo é para que a interface de rede suba automaticamente
auto "nome da interface de rede, ex:> eth0, enp0s3
allow hot-plug enp0s3
iface enp0s3 inet dhcp -> deixe em dhcp ou fixo

#segunda placa de rede 
  auto enp0s8
  iface enp0s8 inet static ou manual
        network 192.168.x.x
        netmask 255.255.x.x
        adresses 192.168.0.x -> ip da maquina a fixar.
        broadcast 192.168.x.255
        
        
        
        
        
        remover tempo de espera grub
edit: /etc/default/grub - comenta a linha timeout
        
        
        
        
        para compartilhar internet devemos ativar o forwading de internet com o comanddo abaixo
 # sysctl -w net.ipv4.ip_forward=" " 1 para ativar, 0 para fechar o compartilhamento de internet
 
 após feito isso, temos de instalar e ativar o netfilter/iptables
 
 apt-get install ufw
ufw enable
 iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
 iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
 iptables -t nat -A POSTROUTING -o "interface" enp0s3 -j MASQUERADE
iptables -t nat -A POSTROUTING -o "interface" enp0s3 -j MASQUERADE

veja se está tudo em ordem 
iptables -L

pronto, primeira parte está ok


