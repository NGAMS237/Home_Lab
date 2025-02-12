# Home_Lab
# 📌 Projet : Configuration d'un Lab Réseau avec Packet Tracer

## 📝 Présentation
Ce projet vise à configurer un réseau local avec plusieurs VLANs, du routage inter-VLAN via un routeur, et des services tels que DHCP et STP. L'objectif est d'apprendre et de documenter la mise en place d'une architecture réseau cohérente.

## 🖥️ Inventaire des équipements
| Type        | Modèle       | Rôle |
|------------|-------------|------|
| Switch 1   | Cisco 3550  | Distribution principale |
| Switch 2   | Cisco 3550  | Extension réseau |
| Routeur    | Cisco 2811  | Routage inter-VLAN |
| PC1        | -           | VLAN 10 - Administration |
| PC2        | -           | VLAN 10 - Administration |
| PC3        | -           | VLAN 20 - Développement |
| PC4        | -           | VLAN 20 - Développement |
| PC5        | -           | VLAN 30 - Finances |
| PC6        | -           | VLAN 40 - RH |
| Serveur DALL | Windows Server | DHCP, DNS, NAS |

## 🌐 Plan d’adressage

| VLAN    | Réseau          | Passerelle        |
|---------|----------------|------------------|
| VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |
| VLAN 40 | 192.168.40.0/24 | 192.168.40.1 |

## 🏗️ Topologie Réseau
Une capture d’écran de la topologie réalisée dans Packet Tracer est incluse pour référence.

## ⚙️ Configuration

### 1️⃣ Création des VLANs
Sur **Switch 1 et Switch 2** :
```
conf t
vlan 10
name Administration
vlan 20
name Developpement
vlan 30
name Finances
vlan 40
name RH
exit
```

### 2️⃣ Configuration des ports d’accès

```
interface fa0/1
switchport mode access
switchport access vlan 10
exit
```
(Same pour les autres VLANs en changeant le numéro du port et du VLAN)

### 3️⃣ Configuration du Trunk entre les switches et le routeur

```
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40
exit
```

### 4️⃣ Activation du STP
```
spanning-tree mode rapid-pvst
spanning-tree vlan 10,20,30,40 priority 4096
```

### 5️⃣ Configuration du routage inter-VLAN sur le routeur

```
interface GigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface GigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface GigabitEthernet0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

interface GigabitEthernet0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
exit
```

### 6️⃣ Configuration du serveur DHCP

Sur le routeur :
```
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.10.2
exit
```
(Répéter pour chaque VLAN)

## ✅ Méthodes de Test et Validation
- **Vérifier les VLANs** sur le switch :
```
show vlan brief
```
- **Vérifier le Trunking** :
```
show interfaces trunk
```
- **Tester la connectivité** avec des pings entre VLANs
- **Vérifier le routage** :
```
show ip route
```

📌 **Fichiers de configuration :**
- `config_switch1.txt`
- `config_switch2.txt`
- `config_router.txt`

🖼️ **Captures d’écran disponibles dans le dossier `screenshots/`**


