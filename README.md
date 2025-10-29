# Atividade2
Configurar roteador CISCO (2911) com VLAN e Servidor DHCP para cada VLAN
A. Passo 1 - Ligação dos componentes.
1. Faça a ligação dos computadores, switch e roteador, conforme a tabela disponibilizada.
B. Passo 2 – Configuração das VLANs no Switch
2. No Switch, em Config, selecione a opção VLAN Database.
3. Adicione as três VLANs, conforme a figura.
4. Ainda em config, selecione as interfaces e insira as VLANs conforme tabela baixo.
Interface
VLAN
FastEthernet0/1 a 0/5
VLAN 10 - Financeiro
FastEthernet0/6 a 0/10
VLAN 20 - Vendas
FastEthernet0/11 a 0/15
VLAN 30 – Diretoria
FastEthernet0/20 a 0/24
VLAN 40 – TI
5. Já a interface GigabitEthernet0/1, vamos deixá-la com opção trunk e deixaremos marcado todas as VLANs que queremos deixar passar por essa interface. Essa ação deve ser realizada, pois queremos que as quatro VLANs acessem o roteador.
C. Passo 3 - Configuração do roteador
1. Vá em CLI no roteador.
2. Coloque o roteador no modo configuração digitando os seguintes comandos:
enable
configure terminal
3. Entre na configuração da interface Gig0/0 digitando o seguinte comando:
int Gig0/0
4. Habilite a interface Gig0/0, digitando o seguinte comando:
no shutdown
5. Crie a subinterface
Digite o comando exit
Para criar a primeira subinterface digite:
int Gig0/0.1
6. Informe informar à subinterface, a qual VLAN ela pertence (VLAN 10), como o comando:
encapsulation dot1Q 10
7. Insira um IP para a subinterface (tem que ser um IP da VLAN). Digite o comando:
ip address 192.168.10.1 255.255.255.0
8. Por padrão, um DHCPDISCOVER (que é broadcast) não sai da VLAN originária. Para que o servidor DHCP (em outra VLAN) receba as requisições, temos que configurar cada subinterface. Para informar o servidor DHCP que irá atender a VLAN de uma interface utilizamos o comando abaixo:
ip helper-address 192.168.40.5
9. É necessário adicionar uma subinterface para cada VLAn. Para adicionar as demais subinterfaces, deixe o roteador no modo de configuração (Router (config)#) e repita os passos a partir do item 5, com o comando int Gig0/0.x (onde o x é a nova subinterface).
10. Digite copy running-config startup-config para salvar as configurações do roteador (você também pode usar wr ou copy r s).
D. Configuração do servidor DHCP
1. Entre no servidor DHCP, Vá em Desktop e Configuração de IP. Insira um IP estático conforme documentado. (IP: 192.168.40.5/24, GW: 192.168.40.1, DNS: 8.8.8.8)
2. Ainda no servidor DHCP, vá em service e depois em DHCP. Cria um pool para cada VLAN, conforme figura abaixo:
3. Verifique se os computadores estão recebendo as configurações de IP de forma dinâmica.
E. Comandos interessantes.
a) Ver as informações sobre todas as interfaces, inclusive seus helper-address.
Router# show ip interface
b) Ver IPs reservados, digite:
show running-config | include excluded-address
c) Ver o status do DHCP no roteador, digite:
show running-config | section dhcp
d) Ver os IPs distribuídos, digite:
show ip dhcp binding
e) Mostrar os pool de DHCP, digite:
show ip dhcp pool
f) Desfazer a reserva de IP, digite:
no ip dhcp excluded-address 192.168.10.1 192.168.10.10
g) Para apagar o pool criado, digite:
Digite config e depois
no ip dhcp pool <nome_do_pool>
