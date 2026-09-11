# 🚀 DevOps

Υλικό γύρω από DevOps πρακτικές, εργαλεία και ροές εργασίας — από τη θεωρία/κουλτούρα μέχρι πλήρη, λειτουργικά CI/CD pipelines. Σχεδιασμένο ώστε κάποιος με background σε system administration (βλ. φακέλους `Windows System Administration & Networking`, `Linux-Administration`) να δει πώς οι δεξιότητές του μεταφέρονται και επεκτείνονται στο DevOps.

---

## 📂 Περιεχόμενα

| Αρχείο | Τι καλύπτει |
|---|---|
| [`devops-fundamentals-roadmap.md`](./devops-fundamentals-roadmap.md) | Τι είναι το DevOps, μοντέλο CALMS, ο DevOps κύκλος (infinity loop), roadmap εκμάθησης, συνολικός χάρτης εργαλείων, και πώς οι sysadmin δεξιότητες μεταφέρονται στο DevOps. **Ξεκίνα από εδώ.** |
| [`devops-git-cicd.md`](./devops-git-cicd.md) | Git σε βάθος (εντολές, branching strategies, merge conflicts), και 3 πλήρη CI/CD pipelines (GitHub Actions, Jenkins, GitLab CI) με deployment strategies και secrets management. |

---

## 🗺️ Προτεινόμενη σειρά ανάγνωσης

```
1. devops-fundamentals-roadmap.md   → Η συνολική εικόνα
2. devops-git-cicd.md               → Version control + αυτοματοποίηση pipelines
```

Επόμενα αρχεία που θα προστεθούν στη σειρά (βλ. roadmap στο πρώτο αρχείο):

- `devops-docker-containers.md` — Containerization βασικά και πρακτική
- `devops-kubernetes.md` — Container orchestration
- `devops-terraform-iac.md` — Infrastructure as Code
- `devops-ansible.md` — Configuration management
- `devops-monitoring-observability.md` — Prometheus, Grafana, logging

---

## 🔗 Σχετικοί φάκελοι στο repo

- [`Windows System Administration & Networking`](../Windows%20System%20Administration%20%26%20Networking) — περιλαμβάνει `azure-iaas-fundamentals.md`, βάση για cloud deployment targets
- [`Linux-Administration`](../Linux-Administration) — Bash scripting, systemd, βάση για CI/CD runners και container hosts
- [`Networking`](../Networking) — δικτυακά θεμέλια απαραίτητα για container/Kubernetes networking
- [`IT-Service-Management`](../IT-Service-Management) — το CI/CD pipeline είναι, ουσιαστικά, αυτοματοποιημένο Change/Release Management

---

*Μέρος του [Infrastructure Knowledge Base](https://github.com/Dimitriskatsanos42/Infrastructure-Knowledge-Base).*
