# Projet_Réseau

# **SAE12** : Mettre en oeuvre un réseau informatique <lr>
<br>
<!--OBJECTIVE-->
<p style="color:red; font-size:30px;">Objectif :</p><BR>
 Cette SAE est un projet qui a pour but de mettre en œuvre un réseau informatique, ce qui implique généralement
<br>
1. Installer et configurer un réseau local (LAN)
<br>
adressage IP<br>
plan d'adressage<br>
configuration de routeurs/switchs<br>
création de sous-réseaux<br>
<br>

2. Mettre en place des services réseaux de base, par exemple :
<br>
DHCP<br>
DNS<br>
serveur web<br>
<br>


3. Sécuriser et tester le réseau
<br>
contrôle d’accès<br>
tests de connectivité (ping, tracert…)<br>
diagnostics réseau<br>



4. Documenter et justifier les choix techniques
<br>
schéma réseau<br>
configuration expliquée<br>
<br>

<!--LISTE EQUIPEMENTS-->

## **<p style="color:red; font-size:25px;">1. Equipement du réseau</p>** <br>
<br>

- **Routeur Cisco série 2911** :
<br>

**Rôle** : Il sert de passerelle par défaut (Gateway). Il gère le routage inter-VLAN (pour que l'Admin accède aux autres réseaux) et le service DHCP (pour distribuer les IP automatiquement).
<br>
<br>
- **Switch Cisco Catalyst 2960** :
<br>
**Rôle** : Il interconnecte tous les équipements. C'est lui qui crée la séparation entre les VLANs (ports d'accès) et qui communique avec le routeur via un lien Trunk.<br>

## **<p style="color:red; font-size:25px;">2. Equipement d'interconnnexion</p>**  <br>
<br>
- **cable Rj45** : Pour permettre l'interconnection des differents équipements
(Rj45 droit & Rj45 croisé)<br>
<br>

## **<p style="color:red; font-size:25px;">3. Equipement de terminaison(Les hôtes)</p>** <br>
<br>

- **VLAN 10** : Admin
- Plage adresse ip : 192.168.10.2-253
- Serveur Web, DNS & FTP : Hébergeant le site intranet (Apache2), la résolution de noms (Bind9) ainsi qu'un service de transfert de fichiers (ProFTPD ou VSFTPD) pour le partage centralisé des ressources de l'entreprise. 
   - IP fixe serveur web : 192.168.10.4
   - IP fixe serveur dns : 192.168.10.5
   - IP fixe serveur FTP : 192.168.10.6
  
  * Apache2 (Le serveur Web)
        C'est le logiciel qui permet d'afficher ton site internet.

      **À quoi ça sert ?** Sans Apache, quand un employé tape l'adresse du site, il ne verrait rien. Apache "donne" les pages web (le texte, les images) aux navigateurs des PC du personnel.

<br>

  * Bind9 (Le serveur DNS)
        C'est l'annuaire du réseau.

      **À quoi ça sert ?** Les ordinateurs ne comprennent que les chiffres (ex: 192.168.10.10). Les humains préfèrent les noms (ex: www.entreprise.local).

      *Quelque commande pour aider à l'installation :
      Avant toute chose, il faut mettre à jour la liste des paquets et installer le service.<br>
            `sudo apt update`<br>
            `sudo apt install bind9 bind9utils bind9-doc`
            
<br> 

- **VLAN 20** : Personnel
 Plage adresse ip : 192.168.20.2-253

- **VLAN 30** : Production 
 Plage adresse ip : 192.168.30.2-253

- **VLAN 40** : Vidéo
 Plage adresse ip : 192.168.40.2-253
- **Serveur NVR** : 
    - Pour le managment des caméras Ip
      Charger de l'enregistrement et le stokage des vidéos sur des disques pour le revisionnages 
      
    - Il centralise la surveillance et évite que les 24 flux ne saturent le reste du réseau.
<br><br>

# Dimensionnement pour le projet

Ce dimensionnement est une estimation fait par moi pour m'aider a avancer sur le projet<br>
- 2 PC Admin (VLAN 10)
- 10 PC Personnel (VLAN 20)
- 3 PC Production (VLAN 30)
- 2 PC Vidéo + 1 NVR + 24 Caméras (VLAN 40)
- 2 PC Wifi (vlan 50) pour SAE 2èm année
<br><br>

## **<p style="color:red; font-size:25px;">4. Commande pour configuration </p>**

# **Configuration Cisco**
<br>

### **<p style="font-size:25px;">Configurationn du switch </p>** 
## Création et Nommage des vlans

enable
configure terminal

### vlan 10
 name ADMIN
### vlan 20
 name PERSONNEL
### vlan 30
 name PRODUCTION
### vlan 40
 name VIDEO
<br>exit</BR> 

## Configuration du lien trunk 
* interface g0/1
 * switchport mode trunk
 * no shutdown
<br>exit</BR>

## Création des VLANs

 **Admin**
* interface range f0/1 - 5
 * switchport mode access
 * switchport access vlan 10
<br>exit</BR>

 **Personnel**
* interface range f0/6 - 15
 * switchport mode access
 * switchport access vlan 20
<br>exit</BR>

 **Production**
* interface range f0/16 - 19
 * switchport mode access
 * switchport access vlan 30
<br>exit</BR>

 **Vidéo**
* interface range f0/20 - 24
 * switchport mode access
 * switchport access vlan 40
<br>exit</BR>
<br>
### **<p style="font-size:20px;">Routeur</p>** 
### **<p style="font-size:18px;">Configurationn des sous interfaces<p>** 

* enable
* configure terminal

### VLAN 10 - ADMIN
**interface g0/0.10**
 * encapsulation dot1Q 10
 * ip address 192.168.10.1 255.255.255.0
 * no shutdown
 * exit

### VLAN 20 - PERSONNEL
**interface g0/0.20**
 * encapsulation dot1Q 20
 * ip address 192.168.20.1 255.255.255.0
 * no shutdown
 * exit

### VLAN 30 - PRODUCTION
**interface g0/0.30**
 * encapsulation dot1Q 30
 * ip address 192.168.30.1 255.255.255.0
 * no shutdown
 * exit

### VLAN 40 - VIDEO
**interface g0/0.40**
 * encapsulation dot1Q 40
 * ip address 192.168.40.1 255.255.255.0
 * no shutdown
 * exit
<br><br>

**Activation de l'interface physique** <br><br>
**interface g0/0**
 * no shutdown
 * exit
<br><br>
## **Image** <br>

### **Image illustrative de la maquette**
![](Maquette.png)<br>
<br>

### **Maquette sur cisco**
![](maquette_cisco.png)<br>
<BR>

### **Diagramme de Gant**
![](gant.png)
<br>
<br>
<br>

<!--INSTALLATION DU SERVEUR APACHE-->
# **5. Configuration en physique**<br>
Port série : /dev/ttyS0<br>
Débits/Parité/Bits : 9600 8N1<br>
Contrôle de flux matériel : Non<br>
Contrôle de flux logiciel : Non<br>

## Configuration du switch et pour l'ACL, ce sont les même commande que Cisco packet tracer<br>

## **Configuration du SSH**<br>
Pour permettre un accès distant sécurisé au périphérique Cisco (switch/routeur) via SSH, au lieu de telnet (non chiffré).<br>

* enable<br>
* configure terminal<br>
* ip domain-name reseau.local<br>
* crypto key generate rsa<br>
* How many bits in the modulus [512]: 2048<br>
* username admin privilege 15 secret [motdepasseadmin]<br>
* line vty 0 4<br>
* transport input ssh<br>
* login local<br>
* exit<br>
* ip ssh version 2<br>
<br>
La connexion sur ce switch peut alors se faire via ssh admin@[@IP_de_l_equipement]<br>

## **Configuration du routeur**
conf t<br>
ip routing<br>

vlan 10<br>
name Admin<br>
vlan 20<br>
name Personnel<br>
vlan 30<br>
name Production<br>
vlan 40<br>
name Vidéo<br>
vlan 50<br>
name Wifi<br>

interface giga0
 switchport mode trunk<br>
 switchport trunk allowed vlan 10,20,30,40,50<br>
 no shutdown<br>

interface vlan 10<br>
 ip address 192.168.10.2 255.255.255.0<br>
 no shutdown<br>

interface vlan 20<br>
 ip address 192.168.20.2 255.255.255.0<br>
 no shutdown<br>

interface vlan 30<br>
 ip address 192.168.30.2 255.255.255.0<br>
 no shutdown<br>

interface vlan 40<br>
 ip address 192.168.40.2 255.255.255.0<br>
 no shutdown<br>

interface vlan 50<br>
 ip address 192.168.50.2 255.255.255.0<br>
 no shutdown<br>

### **Configurationn du serveur apache**
![](APACHE.JPEG)
### **[Documentation utilisée pour l'installation du serveur appache](https://fr.linux-console.net/?p=14996)**

### **Installation de curl**

sudo apt install curl : j'ai dû installer cette commande car le pc ne l'avait reconnue <br>
