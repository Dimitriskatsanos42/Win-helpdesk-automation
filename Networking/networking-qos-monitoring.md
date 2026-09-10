# 📊 QoS, Network Monitoring & Packet Analysis

Τέταρτο αρχείο του φακέλου **Networking**, συμπληρωματικό στα `networking.md`, `networking-practical-labs-cisco.md`, `networking-subnetting-practice.md` και `networking-wireless.md`. Καλύπτει **Quality of Service** (πώς εξασφαλίζεις ότι η κρίσιμη κίνηση έχει προτεραιότητα), **network monitoring** (πώς ξέρεις τι συμβαίνει στο δίκτυο σε πραγματικό χρόνο) και **βασικά packet analysis** με Wireshark.

---

## 🗺️ Πίνακας Περιεχομένων

1. [QoS — Γιατί Χρειάζεται](#-1-qos--γιατί-χρειάζεται)
2. [QoS Μηχανισμοί & Μοντέλα](#-2-qos-μηχανισμοί--μοντέλα)
3. [QoS Marking — DSCP & CoS](#-3-qos-marking--dscp--cos)
4. [Πρακτική QoS Ρύθμιση (Cisco)](#-4-πρακτική-qos-ρύθμιση-cisco)
5. [SNMP — Simple Network Management Protocol](#-5-snmp--simple-network-management-protocol)
6. [NetFlow / sFlow — Ανάλυση Κίνησης](#-6-netflow--sflow--ανάλυση-κίνησης)
7. [Syslog — Κεντρική Καταγραφή](#-7-syslog--κεντρική-καταγραφή)
8. [Wireshark — Βασικά Packet Analysis](#-8-wireshark--βασικά-packet-analysis)
9. [EtherChannel / Link Aggregation](#-9-etherchannel--link-aggregation)
10. [Πλήρες Παράδειγμα — VoIP QoS Design](#-10-πλήρες-παράδειγμα--voip-qos-design)

---

## ⚖️ 1. QoS — Γιατί Χρειάζεται

Σε ένα δίκτυο με περιορισμένο bandwidth, **όχι όλη η κίνηση είναι εξίσου σημαντική**. Το **QoS (Quality of Service)** δίνει προτεραιότητα σε κρίσιμη κίνηση (πχ VoIP, video conferencing) έναντι λιγότερο ευαίσθητης (πχ file download, Windows updates).

### Τα 4 προβλήματα που λύνει το QoS

| Πρόβλημα | Περιγραφή | Επηρεάζει κυρίως |
|---|---|---|
| **Bandwidth** | Δεν υπάρχει αρκετό εύρος για όλους | Όλα |
| **Delay/Latency** | Καθυστέρηση στη μεταφορά πακέτων | VoIP, video calls |
| **Jitter** | Ασυνεπής καθυστέρηση (άλλοτε γρήγορα, άλλοτε αργά) | VoIP — προκαλεί "κοπτόμενη" φωνή |
| **Packet Loss** | Πακέτα που χάνονται στη μεταφορά | Όλα, ειδικά real-time εφαρμογές |

### Ενδεικτικά όρια αποδεκτής ποιότητας για VoIP

| Μετρική | Αποδεκτό όριο |
|---|---|
| Latency (one-way) | <150 ms |
| Jitter | <30 ms |
| Packet Loss | <1% |

---

## 🔧 2. QoS Μηχανισμοί & Μοντέλα

### Μοντέλα QoS

| Μοντέλο | Περιγραφή | Χρήση |
|---|---|---|
| **Best Effort** | Καμία διαφοροποίηση — όλη η κίνηση ίση (default) | Μικρά δίκτυα χωρίς κρίσιμη real-time κίνηση |
| **IntServ** (Integrated Services) | Κάθε flow κάνει reservation bandwidth (RSVP) πριν στείλει δεδομένα | Σπάνια σε production λόγω scalability προβλημάτων |
| **DiffServ** (Differentiated Services) | Η κίνηση κατηγοριοποιείται (marking) και αντιμετωπίζεται ανά κατηγορία | Το πιο διαδεδομένο σε production δίκτυα |

### Οι 3 βασικές λειτουργίες DiffServ (στη σειρά που εφαρμόζονται)
```
1. Classification & Marking (Ταξινόμηση)
   → "Αυτό είναι VoIP κίνηση" → σημαδεύεται με DSCP EF

2. Queuing & Scheduling (Ουρές προτεραιότητας)
   → Η σημαδεμένη κίνηση μπαίνει σε priority queue

3. Congestion Management / Avoidance (Διαχείριση συμφόρησης)
   → Όταν γεμίζει η γραμμή, ποια πακέτα "θυσιάζονται" πρώτα
```

---

## 🏷️ 3. QoS Marking — DSCP & CoS

### DSCP (Differentiated Services Code Point) — Layer 3 marking

| Traffic Class | DSCP Value (Decimal) | Χρήση |
|---|:---:|---|
| **EF** (Expedited Forwarding) | 46 | VoIP (φωνή) — υψηλότερη προτεραιότητα |
| **AF41** (Assured Forwarding) | 34 | Video conferencing |
| **AF31** | 26 | Signaling (πχ SIP call setup) |
| **CS3** | 24 | Network control traffic |
| **Default (Best Effort)** | 0 | Κανονική κίνηση (web, email) |
| **Scavenger** | 8 (CS1) | Χαμηλής προτεραιότητας (πχ backup traffic, torrents) |

### CoS (Class of Service) — Layer 2 marking (802.1p)
- 3-bit πεδίο μέσα στο 802.1Q VLAN tag (τιμές 0-7).
- Χρησιμοποιείται σε trunk links μεταξύ switches, όπου δεν υπάρχει ορατότητα σε Layer 3.

### Trust Boundary
```
[IP Phone] --(marks DSCP EF)--> [Access Switch] --(trusts marking)--> [Core] --> [WAN]
```
> Το **Trust Boundary** ορίζει *πού* στο δίκτυο εμπιστευόμαστε τα markings που έρχονται από τη συσκευή. Συνήθως εμπιστευόμαστε IP phones (γνωστή, ελεγχόμενη συσκευή) αλλά **όχι** τυχαία PCs (θα μπορούσε κάποιος να "ψευτο-σημαδέψει" την κίνησή του ως high-priority).

---

## 🛠️ 4. Πρακτική QoS Ρύθμιση (Cisco)

### Βήμα 1: Class Map (ταξινόμηση κίνησης)
```
Router(config)# class-map match-all VOICE-TRAFFIC
Router(config-cmap)# match dscp ef

Router(config)# class-map match-all VIDEO-TRAFFIC
Router(config-cmap)# match dscp af41
```

### Βήμα 2: Policy Map (τι κάνουμε με κάθε class)
```
Router(config)# policy-map WAN-QOS-POLICY
Router(config-pmap)# class VOICE-TRAFFIC
Router(config-pmap-c)# priority percent 20          # Low-Latency Queue (LLQ)
Router(config-pmap-c)# exit
Router(config-pmap)# class VIDEO-TRAFFIC
Router(config-pmap-c)# bandwidth percent 30
Router(config-pmap-c)# exit
Router(config-pmap)# class class-default
Router(config-pmap-c)# fair-queue
```

### Βήμα 3: Εφαρμογή στο interface
```
Router(config)# interface g0/0
Router(config-if)# service-policy output WAN-QOS-POLICY
```

### Επαλήθευση
```
Router# show policy-map interface g0/0
Router# show class-map
```

### Marking στο access switch port (trust boundary στο IP phone)
```
Switch(config)# interface fa0/1
Switch(config-if)# switchport voice vlan 100
Switch(config-if)# mls qos trust device cisco-phone
```

---

## 📈 5. SNMP — Simple Network Management Protocol

Επιτρέπει σε ένα κεντρικό monitoring σύστημα (πχ PRTG, SolarWinds, Zabbix) να "ρωτάει" (poll) ή να λαμβάνει ειδοποιήσεις (traps) από δικτυακές συσκευές.

### Εκδόσεις SNMP

| Έκδοση | Ασφάλεια | Σχόλιο |
|---|---|---|
| SNMPv1 | Κανένα (community string σε plain text) | Ξεπερασμένο |
| SNMPv2c | Ίδιο με v1, βελτιωμένη απόδοση | Ακόμα ευρέως σε χρήση, αλλά ανασφαλές |
| SNMPv3 | Authentication + Encryption | ✅ Συνιστώμενο για production |

### Βασική έννοια — Polling vs Traps
```
Polling (SNMP GET):
[Monitoring Server] --"Ποια είναι η CPU σου;"--> [Router] --"45%"--> [Monitoring Server]
(Ο monitoring server ρωτάει περιοδικά — πχ κάθε 5 λεπτά)

Traps (SNMP TRAP):
[Router] --"Το interface g0/0 έπεσε!"--> [Monitoring Server]
(Η συσκευή ειδοποιεί μόνη της, άμεσα, χωρίς να περιμένει ερώτηση)
```

### MIB & OID
- **MIB** (Management Information Base): "λεξικό" των διαθέσιμων metrics μιας συσκευής.
- **OID** (Object Identifier): μοναδικός αριθμητικός "δρόμος" προς ένα συγκεκριμένο metric (πχ `1.3.6.1.2.1.1.3.0` = system uptime).

### Ρύθμιση SNMPv3 σε Cisco router
```
Router(config)# snmp-server group MONITOR-GROUP v3 priv
Router(config)# snmp-server user monitor-user MONITOR-GROUP v3 auth sha AuthPass123 priv aes 128 PrivPass123
Router(config)# snmp-server host 192.168.1.50 version 3 priv monitor-user
Router(config)# snmp-server enable traps
```

---

## 🌊 6. NetFlow / sFlow — Ανάλυση Κίνησης

Ενώ το SNMP δίνει **γενικά metrics** (πχ "80% CPU"), το **NetFlow** δίνει **λεπτομέρεια ανά flow** — "ποιος μιλάει με ποιον, τι πρωτόκολλο, πόση κίνηση".

### Τι καταγράφει ένα NetFlow record
```
Source IP: 192.168.10.15
Destination IP: 203.0.113.80
Source Port: 51022
Destination Port: 443
Protocol: TCP
Bytes: 245,000
Duration: 12 sec
```

### Χρήσιμο για
- Εντοπισμός "ποιος/τι καταναλώνει το bandwidth" (πχ ένας χρήστης κάνει torrent).
- Security investigation (ασυνήθιστη κίνηση προς άγνωστο εξωτερικό IP).
- Capacity planning (trends χρήσης bandwidth ανά τμήμα).

### Βασική ρύθμιση (Cisco)
```
Router(config)# interface g0/0
Router(config-if)# ip flow ingress
Router(config-if)# ip flow egress
Router(config-if)# exit

Router(config)# ip flow-export destination 192.168.1.60 2055
Router(config)# ip flow-export version 9
```

---

## 📜 7. Syslog — Κεντρική Καταγραφή

Στέλνει logs από όλες τις δικτυακές συσκευές σε ένα κεντρικό σημείο — απαραίτητο γιατί τα logs σε τοπική μνήμη συσκευής χάνονται σε reboot και είναι δύσκολο να ψάξεις 50 συσκευές ξεχωριστά.

### Syslog Severity Levels

| Level | Όνομα | Παράδειγμα |
|:---:|---|---|
| 0 | Emergency | Σύστημα μη χρησιμοποιήσιμο |
| 1 | Alert | Άμεση ενέργεια απαιτείται |
| 2 | Critical | Κρίσιμη κατάσταση |
| 3 | Error | Σφάλμα λειτουργίας |
| 4 | Warning | Προειδοποίηση |
| 5 | Notice | Φυσιολογικό αλλά σημαντικό γεγονός |
| 6 | Informational | Γενική πληροφόρηση |
| 7 | Debug | Λεπτομέρειες debugging |

### Ρύθμιση σε Cisco συσκευή
```
Router(config)# logging host 192.168.1.60
Router(config)# logging trap warning        # Στέλνει μόνο level 0-4 (πιο σοβαρά)
Router(config)# logging source-interface loopback0
Router(config)# service timestamps log datetime msec
```

---

## 🔬 8. Wireshark — Βασικά Packet Analysis

Το πιο διαδεδομένο εργαλείο για **βαθιά** ανάλυση κίνησης — βλέπεις το περιεχόμενο κάθε πακέτου, byte προς byte.

### Βασικά display filters

| Filter | Τι δείχνει |
|---|---|
| `ip.addr == 192.168.1.10` | Όλη η κίνηση από/προς συγκεκριμένη IP |
| `tcp.port == 443` | Μόνο HTTPS κίνηση |
| `http` | Μόνο HTTP requests/responses |
| `dns` | Μόνο DNS queries/responses |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Μόνο TCP SYN πακέτα (νέες συνδέσεις) |
| `tcp.analysis.retransmission` | Πακέτα που έγιναν retransmit (πιθανό πρόβλημα δικτύου) |
| `icmp` | Ping/traceroute κίνηση |

### Πρακτικό παράδειγμα troubleshooting: "Αργή εφαρμογή"
```
1. Capture κίνηση μεταξύ client και server (filter: ip.addr == <server-ip>)
2. Ψάξε για tcp.analysis.retransmission → αν υπάρχουν πολλά, δείχνει network packet loss
3. Έλεγξε το χρόνο μεταξύ SYN και SYN-ACK (TCP handshake delay) → high latency;
4. Follow TCP Stream (δεξί κλικ → Follow → TCP Stream) → δες την πλήρη συνομιλία
```

### TCP handshake στο Wireshark (τι θα δεις)
```
Frame 1: Client → Server   [SYN]      Seq=0
Frame 2: Server → Client   [SYN, ACK] Seq=0 Ack=1
Frame 3: Client → Server   [ACK]      Seq=1 Ack=1
```

---

## 🔗 9. EtherChannel / Link Aggregation

Συνδυάζει πολλαπλά φυσικά links σε ένα λογικό, για **redundancy** (αν πέσει ένα link, τα υπόλοιπα συνεχίζουν) και **επιπλέον bandwidth**.

### Πρωτόκολλα διαπραγμάτευσης

| Πρωτόκολλο | Τύπος | Σημείωση |
|---|---|---|
| **LACP** (802.3ad) | Ανοιχτό standard | Συνιστώμενο — συμβατό μεταξύ vendors |
| **PAgP** | Cisco proprietary | Μόνο μεταξύ Cisco συσκευών |
| Static (χωρίς πρωτόκολλο) | Χειροκίνητο | Καμία διαπραγμάτευση, ρίσκο misconfiguration |

### Βασική ρύθμιση (LACP)
```
Switch(config)# interface range g0/1 - 2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk

! Επαλήθευση
Switch# show etherchannel summary
Switch# show interfaces port-channel 1
```

---

## 🎯 10. Πλήρες Παράδειγμα — VoIP QoS Design

```
Σενάριο: Εταιρεία με 50 IP phones + video conferencing, μοιράζονται WAN link
          με κανονική κίνηση (web, email, backups)

1. Classification στο access switch:
   - IP Phones: αυτόματο trust μέσω CDP (cisco-phone), DSCP EF
   - Video conferencing app: DSCP AF41
   - Backup traffic (scheduled τη νύχτα): DSCP CS1 (scavenger — χαμηλότερη προτεραιότητα)

2. Policy στο WAN router (interface προς ISP):
   - Voice: Priority Queue (LLQ) 20% εγγυημένο bandwidth
   - Video: 30% εγγυημένο bandwidth
   - Best effort (web/email): 40%
   - Scavenger (backups): 10%, πρώτο να "θυσιαστεί" σε συμφόρηση

3. Monitoring:
   - SNMP polling κάθε 5 λεπτά για interface utilization
   - NetFlow ενεργό στο WAN interface για ανάλυση ποια εφαρμογή καταναλώνει τι
   - Syslog από όλες τις συσκευές σε κεντρικό server

4. Αποτέλεσμα μετά την υλοποίηση:
   - Πριν: παράπονα για "κοπτόμενες" κλήσεις όταν κάποιος κάνει download μεγάλου αρχείου
   - Μετά: Οι κλήσεις παραμένουν καθαρές ανεξάρτητα από άλλη κίνηση,
     επιβεβαιωμένο μέσω Wireshark jitter/latency μετρήσεων
```

---

*Μέρος του [Infrastructure Knowledge Base](https://github.com/Dimitriskatsanos42/Infrastructure-Knowledge-Base) — φάκελος Networking, συμπληρωματικό στα `networking.md`, `networking-practical-labs-cisco.md`, `networking-subnetting-practice.md` και `networking-wireless.md`.*
