# 📶 Wireless Networking — Standards, Security & Design

Συμπληρωματικό αρχείο στα `networking.md`, `networking-practical-labs-cisco.md` και `networking-subnetting-practice.md`. Καλύπτει το ασύρματο δίκτυο — ένα κομμάτι που συχνά λείπει εντελώς από βασικά networking knowledge bases, παρόλο που είναι εξίσου κρίσιμο σε κάθε σύγχρονο περιβάλλον.

---

## 🗺️ Πίνακας Περιεχομένων

1. [Wi-Fi Standards (IEEE 802.11)](#-1-wi-fi-standards-ieee-80211)
2. [Συχνότητες & Κανάλια (2.4GHz vs 5GHz vs 6GHz)](#-2-συχνότητες--κανάλια-24ghz-vs-5ghz-vs-6ghz)
3. [Wireless Security — Εξέλιξη Πρωτοκόλλων](#-3-wireless-security--εξέλιξη-πρωτοκόλλων)
4. [SSID, BSSID & Roaming](#-4-ssid-bssid--roaming)
5. [Wireless Deployment Models](#-5-wireless-deployment-models)
6. [Site Survey & Πλάνο Κάλυψης](#-6-site-survey--πλάνο-κάλυψης)
7. [Interference & Troubleshooting](#-7-interference--troubleshooting)
8. [Πρακτική Ρύθμιση — Cisco WLC/AP Βασικά](#-8-πρακτική-ρύθμιση--cisco-wlcap-βασικά)
9. [Πλήρες Παράδειγμα — Σχεδιασμός WLAN για Γραφείο](#-9-πλήρες-παράδειγμα--σχεδιασμός-wlan-για-γραφείο)

---

## 📡 1. Wi-Fi Standards (IEEE 802.11)

| Standard | Ονομασία Marketing | Συχνότητα | Θεωρητική Ταχύτητα | Χρόνος |
|---|---|---|---:|---|
| 802.11b | — | 2.4 GHz | 11 Mbps | 1999 |
| 802.11g | — | 2.4 GHz | 54 Mbps | 2003 |
| 802.11n | Wi-Fi 4 | 2.4/5 GHz | 600 Mbps | 2009 |
| 802.11ac | Wi-Fi 5 | 5 GHz | 3.5 Gbps | 2014 |
| 802.11ax | Wi-Fi 6 / 6E | 2.4/5/6 GHz | 9.6 Gbps | 2019 |
| 802.11be | Wi-Fi 7 | 2.4/5/6 GHz | ~46 Gbps (θεωρητικά) | 2024 |

### Τι έφερε το Wi-Fi 6 (802.11ax) σε σχέση με το Wi-Fi 5
| Χαρακτηριστικό | Όφελος |
|---|---|
| **OFDMA** (Orthogonal Frequency Division Multiple Access) | Πολλαπλές συσκευές εξυπηρετούνται ταυτόχρονα στο ίδιο κανάλι, όχι σειριακά |
| **MU-MIMO** (βελτιωμένο, uplink+downlink) | Ταυτόχρονη επικοινωνία με πολλαπλές συσκευές |
| **TWT** (Target Wake Time) | Εξοικονόμηση μπαταρίας σε IoT/κινητές συσκευές |
| **BSS Coloring** | Μείωση interference σε πυκνά περιβάλλοντα (πολλά APs κοντά) |

---

## 📊 2. Συχνότητες & Κανάλια (2.4GHz vs 5GHz vs 6GHz)

| Χαρακτηριστικό | 2.4 GHz | 5 GHz | 6 GHz |
|---|---|---|---|
| Εμβέλεια | Μεγαλύτερη (διαπερνά τοίχους καλύτερα) | Μικρότερη | Μικρότερη ακόμα |
| Ταχύτητα | Χαμηλότερη | Υψηλότερη | Υψηλότερη |
| Διαθέσιμα non-overlapping κανάλια | Μόνο 3 (1, 6, 11) | 20+ | Πολύ περισσότερα |
| Παρεμβολές (interference) | Υψηλές (Bluetooth, φούρνοι μικροκυμάτων, γείτονες) | Χαμηλότερες | Ελάχιστες (νέο, λιγότερο "γεμάτο") |
| Καλύτερο για | IoT συσκευές, μεγάλη εμβέλεια | Γενική χρήση, laptops/κινητά | High-bandwidth apps, νέες συσκευές Wi-Fi 6E/7 |

### Γιατί μόνο τα κανάλια 1, 6, 11 στο 2.4GHz;
Τα κανάλια στο 2.4GHz επικαλύπτονται (overlap) μεταξύ τους αν είναι κοντά. Μόνο τα 1, 6 και 11 είναι αρκετά απομακρυσμένα ώστε να **μην** επικαλύπτονται — χρήση οποιουδήποτε άλλου καναλιού (πχ 4, 9) προκαλεί παρεμβολές με τα γειτονικά.

```
Κανάλι:     1        6        11
Frequency:  2412MHz  2437MHz  2462MHz
            [--22MHz--]
                     [--22MHz--]
                              [--22MHz--]
```

---

## 🔐 3. Wireless Security — Εξέλιξη Πρωτοκόλλων

| Πρωτόκολλο | Έτος | Κρυπτογράφηση | Κατάσταση |
|---|---|---|---|
| WEP | 1997 | RC4 (40/104-bit) | ❌ Εντελώς ξεπερασμένο, σπάει σε λεπτά |
| WPA | 2003 | TKIP | ❌ Ξεπερασμένο |
| WPA2 | 2004 | AES-CCMP | ⚠️ Ακόμα σε χρήση, αλλά ευάλωτο σε KRACK attack |
| WPA3 | 2018 | AES-GCMP, SAE | ✅ Τρέχον standard, συνιστάται |

### WPA2-Personal vs WPA2/3-Enterprise

| | Personal (PSK) | Enterprise (802.1X) |
|---|---|---|
| Authentication | Κοινός κωδικός για όλους | Individual credentials (username/password ή certificate) ανά χρήστη |
| Χρήση | Οικιακά δίκτυα, μικρά γραφεία | Εταιρικά περιβάλλοντα |
| Απαιτεί | Τίποτα επιπλέον | RADIUS server (βλ. `windows-virtualization-clustering-infrastructure.md`) |
| Ανάκληση πρόσβασης | Πρέπει να αλλάξεις τον κοινό κωδικό (επηρεάζει ΟΛΟΥΣ) | Απενεργοποίηση μεμονωμένου λογαριασμού |

### WPA3 — τι βελτιώνει
- **SAE (Simultaneous Authentication of Equals)**: αντικαθιστά το PSK 4-way handshake, προστατεύει από offline dictionary attacks.
- **Forward Secrecy**: ακόμα κι αν κάποιος υποκλέψει τον κωδικό αργότερα, δεν μπορεί να αποκρυπτογραφήσει παλιότερη κίνηση.
- **Enhanced Open**: κρυπτογράφηση ακόμα και σε ανοιχτά (χωρίς κωδικό) δίκτυα, πχ public Wi-Fi.

---

## 🏷️ 4. SSID, BSSID & Roaming

| Όρος | Σημασία |
|---|---|
| **SSID** (Service Set Identifier) | Το "όνομα δικτύου" που βλέπει ο χρήστης (πχ "Contoso-WiFi") |
| **BSSID** | Η μοναδική MAC address του συγκεκριμένου Access Point |
| **ESSID** | Πολλά APs μοιράζονται το ίδιο SSID, δημιουργώντας ενιαία κάλυψη σε όλο το κτίριο |

### Seamless Roaming
Όταν ο χρήστης κινείται και το laptop/κινητό αλλάζει AP χωρίς να χάσει τη σύνδεση:
- **802.11r (Fast BSS Transition)**: επιταχύνει το roaming handoff, κρίσιμο για VoIP over Wi-Fi.
- **802.11k**: το AP δίνει στη συσκευή λίστα γειτονικών APs, ώστε να αποφασίσει γρηγορότερα πού να μεταπηδήσει.
- **802.11v**: το δίκτυο μπορεί να "καθοδηγήσει" ενεργά τη συσκευή προς καλύτερο AP.

---

## 🏗️ 5. Wireless Deployment Models

| Μοντέλο | Περιγραφή | Χρήση |
|---|---|---|
| **Autonomous AP** | Κάθε AP λειτουργεί ανεξάρτητα, με δική του ρύθμιση | Πολύ μικρά περιβάλλοντα |
| **Controller-based (WLC)** | Κεντρικός Wireless LAN Controller διαχειρίζεται όλα τα APs | Εταιρικά περιβάλλοντα, εύκολη κεντρική διαχείριση |
| **Cloud-managed** (πχ Meraki, Aruba Central) | Διαχείριση μέσω cloud dashboard, χωρίς on-prem controller | Multi-site επιχειρήσεις, απομακρυσμένη διαχείριση |
| **Mesh** | APs επικοινωνούν μεταξύ τους ασύρματα (χωρίς καλώδιο σε κάθε ένα) | Εξωτερικοί χώροι, δύσκολη καλωδίωση |

---

## 📐 6. Site Survey & Πλάνο Κάλυψης

### Γιατί χρειάζεται Site Survey πριν την εγκατάσταση
- Εντοπισμός **dead zones** (σημεία χωρίς κάλυψη).
- Αποφυγή **co-channel interference** (πολλά APs στο ίδιο κανάλι που "ακούγονται" μεταξύ τους).
- Σωστή τοποθέτηση APs βάσει υλικών κατασκευής (μπετόν vs γυψοσανίδα επηρεάζουν διαφορετικά το σήμα).

### Βασικές μετρήσεις σε site survey

| Μετρική | Καλή τιμή | Σημασία |
|---|---|---|
| **RSSI** (Received Signal Strength) | -30 έως -67 dBm | Ισχύς σήματος (πιο κοντά στο 0 = καλύτερο) |
| **SNR** (Signal-to-Noise Ratio) | >25 dB | Πόσο "καθαρό" είναι το σήμα σε σχέση με θόρυβο |
| **Channel Utilization** | <50% | Πόσο "γεμάτο" είναι το κανάλι |

### Απλός κανόνας τοποθέτησης AP
- Overlap κάλυψης ~15-20% μεταξύ γειτονικών APs (για ομαλό roaming, χωρίς κενά).
- Γειτονικά APs σε **διαφορετικά, μη επικαλυπτόμενα κανάλια** (βλ. κεφάλαιο 2).

---

## 🔍 7. Interference & Troubleshooting

### Πηγές παρεμβολών (interference)
| Πηγή | Επηρεάζει |
|---|---|
| Φούρνος μικροκυμάτων | 2.4 GHz |
| Bluetooth συσκευές | 2.4 GHz |
| Ασύρματα τηλέφωνα (παλιά DECT) | 2.4/5.8 GHz |
| Γειτονικά δίκτυα (co-channel) | Οποιαδήποτε συχνότητα |
| Φυσικά εμπόδια (μέταλλο, μπετόν) | Εξασθένηση σήματος (attenuation) |

### Troubleshooting flow για "αργό/ασταθές Wi-Fi"
```
1. Επιβεβαίωση: πρόβλημα Wi-Fi ή πρόβλημα upstream (internet, DHCP);
   → Ping στο gateway, μετά ping σε εξωτερικό (8.8.8.8)

2. Έλεγχος signal strength στη συσκευή (RSSI)
   → Αν πολύ αδύναμο: πρόβλημα κάλυψης, όχι interference

3. Έλεγχος channel utilization στον controller/AP
   → Αν υψηλό: πολλές συσκευές/κίνηση στο ίδιο κανάλι

4. Έλεγχος για co-channel interference από γειτονικά δίκτυα
   → WiFi analyzer app (πχ NetSpot, inSSIDer)

5. Έλεγχος αν το πρόβλημα είναι roaming-related
   → Συμβαίνει μόνο όταν ο χρήστης κινείται μεταξύ ορόφων/χώρων;
```

---

## 🛠️ 8. Πρακτική Ρύθμιση — Cisco WLC/AP Βασικά

### Βασική δημιουργία WLAN (SSID) σε Cisco WLC (CLI concept)
```
# Δημιουργία νέου WLAN
(WLC) > config wlan create 1 Contoso-Corp Contoso-Corp

# Ρύθμιση security (WPA2-Enterprise με RADIUS)
(WLC) > config wlan security wpa akm 802.1X enable 1
(WLC) > config wlan radius_server auth add 1 <radius-server-ip> 1812 ascii <shared-secret>

# Ανάθεση σε VLAN
(WLC) > config wlan interface 1 VLAN10-Corp

# Ενεργοποίηση
(WLC) > config wlan enable 1
```

### Guest Network (απομονωμένο, χωρίς πρόσβαση σε εσωτερικό δίκτυο)
```
(WLC) > config wlan create 2 Contoso-Guest Contoso-Guest
(WLC) > config wlan security wpa akm psk enable 2
(WLC) > config wlan security wpa akm psk set-key ascii <guest-password> 2
(WLC) > config wlan interface 2 VLAN99-Guest
```
> Το Guest VLAN πρέπει να είναι **δικτυακά απομονωμένο** από το εσωτερικό δίκτυο (μέσω ACL/firewall rule) — οι επισκέπτες παίρνουν internet, όχι πρόσβαση σε εσωτερικούς πόρους.

---

## 🎯 9. Πλήρες Παράδειγμα — Σχεδιασμός WLAN για Γραφείο

```
Σενάριο: Γραφείο 2 ορόφων, 80 εργαζόμενοι, ανάγκη για corporate + guest Wi-Fi

1. Site Survey:
   - Εντοπισμός ότι ο 1ος όροφος έχει μπετόν τοίχους (χρειάζεται περισσότερα APs)
   - Ο 2ος όροφος έχει ανοιχτό open-plan σχεδιασμό (λιγότερα APs αρκούν)

2. Deployment model: Controller-based (WLC) — κεντρική διαχείριση και για τους 2 ορόφους

3. WLANs που δημιουργούνται:
   - "Contoso-Corp" (WPA2/3-Enterprise, 802.1X μέσω NPS/RADIUS) → VLAN 10
   - "Contoso-Guest" (WPA2-Personal, captive portal) → VLAN 99 (απομονωμένο)
   - "Contoso-IoT" (WPA2-Personal, ξεχωριστό VLAN) → VLAN 50 (εκτυπωτές, κάμερες)

4. Κανάλια:
   - 2.4GHz: εναλλαγή 1/6/11 μεταξύ γειτονικών APs
   - 5GHz: χρήση μη επικαλυπτόμενων καναλιών (πολύ περισσότερα διαθέσιμα)

5. Roaming: Ενεργοποίηση 802.11r/k/v για ομαλή μετάβαση μεταξύ ορόφων

6. Ασφάλεια:
   - Corporate SSID: κάθε χρήστης authenticate με domain credentials (μέσω RADIUS)
   - Guest SSID: captive portal με αποδοχή terms of use, time-limited access

7. Μετά την υλοποίηση: δεύτερο site survey (validation) για επιβεβαίωση κάλυψης
```

---

*Μέρος του [Infrastructure Knowledge Base](https://github.com/Dimitriskatsanos42/Infrastructure-Knowledge-Base) — φάκελος Networking, συμπληρωματικό στα `networking.md`, `networking-practical-labs-cisco.md` και `networking-subnetting-practice.md`.*
