# 🚀 DevOps Fundamentals — Τι Πρέπει να Ξέρεις

Πρώτο αρχείο νέου φακέλου **DevOps**. Δίνει τη **συνολική εικόνα**: τι είναι το DevOps, ποια είναι τα βασικά πεδία γνώσης, και πώς όλα συνδέονται. Τα υπόλοιπα αρχεία του φακέλου θα μπουν σε βάθος σε κάθε πεδίο ξεχωριστά.

---

## 🗺️ Πίνακας Περιεχομένων

1. [Τι Είναι το DevOps — Πέρα από τον Ορισμό](#-1-τι-είναι-το-devops--πέρα-από-τον-ορισμό)
2. [Το Μοντέλο CALMS](#-2-το-μοντέλο-calms)
3. [Ο DevOps Κύκλος (Infinity Loop)](#-3-ο-devops-κύκλος-infinity-loop)
4. [Βασικά Πεδία Γνώσης — Roadmap](#-4-βασικά-πεδία-γνώσης--roadmap)
5. [Version Control — Το Θεμέλιο](#-5-version-control--το-θεμέλιο)
6. [CI/CD — Η Καρδιά του DevOps](#-6-cicd--η-καρδιά-του-devops)
7. [Infrastructure as Code (IaC)](#-7-infrastructure-as-code-iac)
8. [Containerization & Orchestration](#-8-containerization--orchestration)
9. [Configuration Management](#-9-configuration-management)
10. [Monitoring & Observability](#-10-monitoring--observability)
11. [Εργαλεία — Συνολικός Χάρτης](#-11-εργαλεία--συνολικός-χάρτης)
12. [Πώς Συνδέεται με ό,τι Ήδη Ξέρεις (Sysadmin → DevOps)](#-12-πώς-συνδέεται-με-ό-τι-ήδη-ξέρεις-sysadmin--devops)

---

## 💡 1. Τι Είναι το DevOps — Πέρα από τον Ορισμό

Το DevOps **δεν είναι εργαλείο ή θέση εργασίας** — είναι **κουλτούρα και σύνολο πρακτικών** που ενώνει το **Development** (γράψιμο κώδικα) με τα **Operations** (λειτουργία υποδομής), ώστε το λογισμικό να παραδίδεται **γρηγορότερα, πιο αξιόπιστα και με λιγότερο τριβή** μεταξύ ομάδων.

### Το πρόβλημα που λύνει
```
Παλιό μοντέλο (Silos):
Developers γράφουν κώδικα → "πετάνε" το build στο Ops → Ops προσπαθεί να το τρέξει
        ↓
"Στο μηχάνημά μου δουλεύει" — Ops δεν ξέρει γιατί κολλάει σε production
        ↓
Αργές, σπάνιες, ρισκαρισμένες releases (πχ 1 φορά/τρίμηνο, με πολλά προβλήματα)

DevOps μοντέλο:
Developers + Ops συνεργάζονται από την αρχή, με αυτοματοποιημένα pipelines
        ↓
Συχνές, μικρές, χαμηλού ρίσκου releases (πχ πολλαπλές φορές/ημέρα)
```

### Sysadmin vs DevOps — η εξέλιξη του ρόλου
| Παραδοσιακός Sysadmin | DevOps / SRE |
|---|---|
| Χειροκίνητη ρύθμιση servers | Infrastructure as Code (τα πάντα σε κώδικα) |
| Reactive (διορθώνει μετά το πρόβλημα) | Proactive (monitoring, alerting, auto-remediation) |
| Ξεχωριστό silo από τους developers | Στενή συνεργασία, κοινή ευθύνη ("you build it, you run it") |
| Manual deployment | Automated CI/CD pipelines |

---

## 🧭 2. Το Μοντέλο CALMS

Πλαίσιο για να καταλάβεις τις 5 διαστάσεις της DevOps κουλτούρας:

| Γράμμα | Έννοια | Τι σημαίνει στην πράξη |
|:---:|---|---|
| **C** | Culture | Συνεργασία αντί για ρίψη ευθύνης, ψυχολογική ασφάλεια για λάθη |
| **A** | Automation | Αυτοματοποίηση επαναλαμβανόμενων εργασιών (deployments, testing, provisioning) |
| **L** | Lean | Μικρά, συχνά βήματα αντί για τεράστια, σπάνια releases |
| **M** | Measurement | Metrics για ΟΛΑ (deployment frequency, error rates, response time) |
| **S** | Sharing | Κοινή γνώση, τεκμηρίωση, δεν κρατάει κανείς "μαγικά" tricks μόνο για τον εαυτό του |

---

## ♾️ 3. Ο DevOps Κύκλος (Infinity Loop)

```
     Plan → Code → Build → Test
      ↑                        ↓
   Monitor ← Operate ← Release ← Deploy
```

| Φάση | Τι γίνεται | Τυπικά εργαλεία |
|---|---|---|
| **Plan** | Requirements, backlog, sprint planning | Jira, Azure DevOps Boards |
| **Code** | Ανάπτυξη, version control | Git, GitHub/GitLab |
| **Build** | Compile, package | Maven, npm, Docker build |
| **Test** | Automated testing | Jest, Selenium, pytest |
| **Release** | Έγκριση/προετοιμασία για production | GitHub Actions, Jenkins |
| **Deploy** | Πραγματική εγκατάσταση σε παραγωγή | Kubernetes, Ansible, Terraform |
| **Operate** | Λειτουργία σε production | Kubernetes, cloud platforms |
| **Monitor** | Παρακολούθηση υγείας/απόδοσης | Prometheus, Grafana, Datadog |

> Ο κύκλος είναι **συνεχής** — τα ευρήματα από το Monitor τροφοδοτούν το επόμενο Plan.

---

## 🗺️ 4. Βασικά Πεδία Γνώσης — Roadmap

Προτεινόμενη σειρά εκμάθησης (κάθε πεδίο θα αναλυθεί σε ξεχωριστό αρχείο του φακέλου):

```
1. Version Control (Git) ─────────────── Θεμέλιο, χωρίς αυτό τίποτα άλλο δεν δουλεύει
        ↓
2. Linux & Scripting Βασικά ──────────── (βλ. ήδη τον φάκελο Linux-Administration)
        ↓
3. CI/CD Pipelines ────────────────────── Αυτοματοποίηση build/test/deploy
        ↓
4. Containerization (Docker) ──────────── Πακετάρισμα εφαρμογών
        ↓
5. Container Orchestration (Kubernetes) ─ Διαχείριση containers σε scale
        ↓
6. Infrastructure as Code (Terraform) ─── Υποδομή σαν κώδικας
        ↓
7. Configuration Management (Ansible) ─── Ρύθμιση servers μέσω κώδικα
        ↓
8. Cloud Platforms (Azure/AWS) ────────── (βλ. ήδη azure-iaas-fundamentals.md)
        ↓
9. Monitoring & Observability ─────────── Prometheus/Grafana, logging
        ↓
10. Security (DevSecOps) ──────────────── Ασφάλεια ενσωματωμένη σε όλα τα παραπάνω
```

---

## 🌳 5. Version Control — Το Θεμέλιο

Χωρίς σωστό version control, τίποτα άλλο στο DevOps δεν έχει νόημα — είναι η βάση πάνω στην οποία χτίζεται το CI/CD.

### Βασικές έννοιες (θα αναλυθούν πλήρως σε ξεχωριστό αρχείο)
| Έννοια | Περιγραφή |
|---|---|
| **Repository** | Ο "φάκελος" που κρατά όλη την ιστορία του κώδικα |
| **Commit** | Ένα "στιγμιότυπο" αλλαγών με μήνυμα εξήγησης |
| **Branch** | Παράλληλη γραμμή ανάπτυξης, απομονωμένη από το main |
| **Merge/Pull Request** | Η διαδικασία ενσωμάτωσης αλλαγών πίσω στο main |
| **Tag** | Σήμανση συγκεκριμένου commit ως "release" (πχ v1.2.0) |

### Branching Strategies (θα αναλυθούν στο επόμενο αρχείο)
- **Git Flow**: πολύπλοκο, πολλά branches (feature, develop, release, hotfix)
- **GitHub Flow**: απλό, μόνο main + feature branches
- **Trunk-Based Development**: όλοι δουλεύουν σχεδόν πάντα στο main, με πολύ συχνά, μικρά commits

---

## 🔄 6. CI/CD — Η Καρδιά του DevOps

| Όρος | Σημασία |
|---|---|
| **CI** (Continuous Integration) | Κάθε αλλαγή κώδικα ενσωματώνεται αυτόματα, με automated tests που τρέχουν σε κάθε commit |
| **CD** (Continuous Delivery) | Κάθε αλλαγή που περνάει τα tests είναι **έτοιμη** για production (αλλά το deployment μπορεί να είναι manual trigger) |
| **CD** (Continuous Deployment) | Κάθε αλλαγή που περνάει τα tests πηγαίνει **αυτόματα** σε production, χωρίς ανθρώπινη παρέμβαση |

### Τυπικό pipeline
```
Developer κάνει push κώδικα
        ↓
CI Server ανιχνεύει την αλλαγή (webhook)
        ↓
1. Build (compile/package)
        ↓
2. Unit Tests
        ↓
3. Static Code Analysis / Linting
        ↓
4. Security Scan (SAST)
        ↓
5. Integration Tests
        ↓
6. Build Docker Image
        ↓
7. Deploy σε Staging
        ↓
8. Automated Acceptance Tests
        ↓
9. (Approval Gate αν Continuous Delivery, όχι Deployment)
        ↓
10. Deploy σε Production
```

> Θα δεις πλήρη, πρακτικά παραδείγματα (GitHub Actions, Jenkins) στο επόμενο αρχείο της σειράς.

---

## 🏗️ 7. Infrastructure as Code (IaC)

Αντί να ρυθμίζεις servers χειροκίνητα (κλικ σε GUI, ή ακόμα και manual PowerShell/Bash commands), **γράφεις την υποδομή σε κώδικα** — δηλωτικά (declarative), αναπαραγώγιμα, version-controlled.

### Γιατί έχει σημασία
| Χωρίς IaC | Με IaC |
|---|---|
| "Ξέρω πώς το έστησα, αλλά δεν το έχω γραμμένο πουθενά" | Ο κώδικας ΕΙΝΑΙ η τεκμηρίωση |
| Κάθε περιβάλλον (dev/staging/prod) είναι λίγο διαφορετικό ("configuration drift") | Ακριβώς ίδια, αναπαραγώγιμη υποδομή παντού |
| Disaster recovery = ελπίζεις να θυμάσαι όλα τα βήματα | Disaster recovery = τρέχεις ξανά τον κώδικα |

### Βασικά εργαλεία (θα αναλυθούν σε ξεχωριστό αρχείο)
| Εργαλείο | Τύπος | Focus |
|---|---|---|
| **Terraform** | Provisioning (δημιουργία resources) | Cloud-agnostic, πολλαπλοί providers |
| **Azure Bicep / ARM Templates** | Provisioning | Azure-specific |
| **AWS CloudFormation** | Provisioning | AWS-specific |
| **Ansible** | Configuration Management | Ρύθμιση ήδη υπαρχόντων servers |

---

## 🐳 8. Containerization & Orchestration

### Γιατί containers (σύντομη υπενθύμιση — αναλύεται πλήρως σε ξεχωριστό αρχείο)
Πακετάρουν την εφαρμογή **μαζί με όλες τις εξαρτήσεις της** σε μια φορητή μονάδα — τρέχει ίδια παντού (dev laptop, staging, production, οποιοδήποτε cloud).

### Docker vs Kubernetes — η διαφορά
| | Docker | Kubernetes |
|---|---|---|
| Τι είναι | Δημιουργεί/τρέχει μεμονωμένα containers | Ορχηστρώνει (διαχειρίζεται) πολλά containers σε scale |
| Ανάλογο | Ένα φορτηγό | Ολόκληρο λιμάνι με εκατοντάδες φορτηγά, δρομολόγηση, συντήρηση |
| Χρήση | Local development, μικρά deployments | Production, μεγάλης κλίμακας, high availability |

---

## ⚙️ 9. Configuration Management

Διαφορά από το IaC provisioning: το **IaC δημιουργεί** την υποδομή (VM, network), το **Configuration Management ρυθμίζει** τι τρέχει μέσα σε αυτή (packages, files, services).

| Εργαλείο | Προσέγγιση | Σημείωση |
|---|---|---|
| **Ansible** | Agentless (μέσω SSH/WinRM) | Πιο απλό ξεκίνημα, δημοφιλές |
| **Puppet** | Agent-based | Πιο ώριμο, enterprise-heavy |
| **Chef** | Agent-based, Ruby DSL | Λιγότερο δημοφιλές πλέον |
| **PowerShell DSC** | Native Windows | Καλή επιλογή αν είσαι ήδη σε Windows-heavy περιβάλλον |

---

## 📡 10. Monitoring & Observability

### Τα "3 Pillars" του Observability
| Pillar | Απαντά | Εργαλεία παράδειγμα |
|---|---|---|
| **Metrics** | "Πόσο;" (CPU%, request rate, latency) | Prometheus, Grafana |
| **Logs** | "Τι ακριβώς συνέβη;" | ELK Stack (Elasticsearch/Logstash/Kibana), Loki |
| **Traces** | "Πού πήγε το request μέσα από πολλαπλά microservices;" | Jaeger, Zipkin |

> Στο ITSM υλικό σου (`itsm-event-availability-policies.md`) κάλυψες ήδη το **Event Management** — το DevOps monitoring είναι η τεχνική υλοποίηση αυτής της ίδιας ιδέας, σε πιο cloud-native/microservices context.

---

## 🧰 11. Εργαλεία — Συνολικός Χάρτης

| Κατηγορία | Δημοφιλή εργαλεία |
|---|---|
| Version Control | Git, GitHub, GitLab, Bitbucket |
| CI/CD | GitHub Actions, GitLab CI, Jenkins, Azure DevOps Pipelines |
| Containers | Docker, Podman |
| Orchestration | Kubernetes, Docker Swarm |
| IaC | Terraform, Ansible, Pulumi |
| Configuration Mgmt | Ansible, Puppet, Chef |
| Cloud | Azure, AWS, GCP |
| Monitoring | Prometheus, Grafana, Datadog, New Relic |
| Logging | ELK Stack, Splunk, Loki |
| Secrets Management | HashiCorp Vault, Azure Key Vault, AWS Secrets Manager |
| Artifact Repository | Nexus, Artifactory, Docker Hub, GitHub Packages |

---

## 🔗 12. Πώς Συνδέεται με ό,τι Ήδη Ξέρεις (Sysadmin → DevOps)

Το πολύ καλό νέο: **δεν ξεκινάς από το μηδέν**. Πολλά από όσα έχεις ήδη στο knowledge base σου μεταφέρονται άμεσα:

| Τι ήδη ξέρεις | Πώς μεταφέρεται στο DevOps |
|---|---|
| PowerShell/Bash scripting | Βάση για Ansible playbooks, CI/CD scripts |
| Linux administration | Απαραίτητο — τα περισσότερα containers/K8s nodes τρέχουν Linux |
| Networking (VLANs, subnetting) | Kubernetes networking, container networking βασίζονται στα ίδια θεμέλια |
| Azure IaaS | Φυσικό επόμενο βήμα προς Azure DevOps/AKS (Kubernetes managed service) |
| ITSM (Change Management) | Το CI/CD **είναι** αυτοματοποιημένο Change Management |
| Windows/Linux troubleshooting | Ίδια νοοτροπία χρειάζεται για debugging containers/pipelines |

> Το DevOps δεν αντικαθιστά τις sysadmin δεξιότητες — τις **χτίζει πάνω** με αυτοματοποίηση και νέα εργαλεία.

---

*Μέρος του [Infrastructure Knowledge Base](https://github.com/Dimitriskatsanos42/Infrastructure-Knowledge-Base) — νέος φάκελος DevOps. Πρώτο αρχείο μιας σειράς που θα καλύψει Git/CI-CD, Docker, Kubernetes, Terraform και Ansible σε βάθος.*
