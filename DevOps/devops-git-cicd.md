# 🌳 Git & CI/CD — Πρακτικές Εντολές & Pipelines

Δεύτερο αρχείο του φακέλου **DevOps**, συμπληρωματικό στο `devops-fundamentals-roadmap.md`. Καλύπτει **Git σε βάθος** (εντολές, branching, συνηθισμένα προβλήματα) και **πλήρη, λειτουργικά CI/CD pipelines** σε GitHub Actions και Jenkins.

---

## 🗺️ Πίνακας Περιεχομένων

1. [Git — Βασικές Εντολές](#-1-git--βασικές-εντολές)
2. [Branching & Merging](#-2-branching--merging)
3. [Git — Συνηθισμένα Προβλήματα & Λύσεις](#-3-git--συνηθισμένα-προβλήματα--λύσεις)
4. [Branching Strategies σε Βάθος](#-4-branching-strategies-σε-βάθος)
5. [GitHub Actions — Πλήρες Pipeline](#-5-github-actions--πλήρες-pipeline)
6. [Jenkins — Πλήρες Pipeline (Jenkinsfile)](#-6-jenkins--πλήρες-pipeline-jenkinsfile)
7. [GitLab CI — Πλήρες Pipeline](#-7-gitlab-ci--πλήρες-pipeline)
8. [Deployment Strategies](#-8-deployment-strategies)
9. [Secrets Management σε Pipelines](#-9-secrets-management-σε-pipelines)
10. [Πλήρες Παράδειγμα — Από Commit σε Production](#-10-πλήρες-παράδειγμα--από-commit-σε-production)

---

## 📝 1. Git — Βασικές Εντολές

### Setup & Configuration
```bash
git config --global user.name "Το Όνομά σου"
git config --global user.email "email@example.com"
git config --global init.defaultBranch main
```

### Δημιουργία/Κλωνοποίηση Repository
```bash
git init                                    # Νέο repo τοπικά
git clone https://github.com/user/repo.git  # Αντιγραφή υπάρχοντος repo
```

### Καθημερινή ροή εργασίας
```bash
git status                     # Τι έχει αλλάξει
git add file.txt                # Προσθήκη συγκεκριμένου αρχείου στο staging
git add .                       # Προσθήκη όλων των αλλαγών
git commit -m "Περιγραφή αλλαγής"
git push origin main            # Αποστολή στο remote repository
git pull origin main            # Λήψη τελευταίων αλλαγών από remote
```

### Ιστορικό & Επιθεώρηση
```bash
git log                         # Πλήρες ιστορικό commits
git log --oneline --graph --all # Συμπυκνωμένη, οπτική προβολή branches
git diff                        # Τι άλλαξε (μη-staged)
git diff --staged               # Τι άλλαξε (staged, έτοιμο για commit)
git show <commit-hash>          # Λεπτομέρειες συγκεκριμένου commit
```

---

## 🌿 2. Branching & Merging

```bash
git branch                      # Λίστα τοπικών branches
git branch feature/login-page   # Δημιουργία νέου branch
git checkout feature/login-page # Μετάβαση σε branch
git checkout -b feature/new-api # Δημιουργία + μετάβαση ταυτόχρονα

# Νεότερη σύνταξη (Git 2.23+)
git switch feature/login-page
git switch -c feature/new-api
```

### Merge
```bash
git checkout main
git merge feature/login-page    # Ενσωμάτωση feature branch στο main
```

### Rebase (εναλλακτικό στο merge — "καθαρότερο" ιστορικό)
```bash
git checkout feature/login-page
git rebase main                 # Ξαναγράφει τα commits του branch πάνω στο τρέχον main
```

### Merge vs Rebase — πότε το ένα, πότε το άλλο
| | Merge | Rebase |
|---|---|---|
| Ιστορικό | Διατηρεί πλήρες, "θορυβώδες" ιστορικό με merge commits | Γραμμικό, "καθαρό" ιστορικό |
| Ασφάλεια | Ασφαλές πάντα | ⚠️ Ποτέ σε **shared/public** branches — ξαναγράφει ιστορικό |
| Χρήση | Merging feature branches σε main | Ενημέρωση προσωπικού feature branch με τελευταίες αλλαγές από main |

---

## 🔧 3. Git — Συνηθισμένα Προβλήματα & Λύσεις

### "Έκανα commit στο λάθος branch"
```bash
git log                          # Βρες το commit hash
git reset --soft HEAD~1          # Αναίρεση του τελευταίου commit (κρατάει τις αλλαγές)
git stash                        # Προσωρινή αποθήκευση αλλαγών
git checkout σωστό-branch
git stash pop                    # Επαναφορά αλλαγών στο σωστό branch
git add . && git commit -m "..."
```

### "Θέλω να αναιρέσω ένα commit που ήδη έκανα push"
```bash
git revert <commit-hash>         # Δημιουργεί ΝΕΟ commit που αναιρεί το προηγούμενο
                                  # (ασφαλές — δεν ξαναγράφει ιστορικό)
git push origin main
```

### Merge Conflict — πώς λύνεται
```bash
git merge feature-branch
# CONFLICT (content): Merge conflict in file.txt

# Άνοιξε το αρχείο, θα δεις:
<<<<<<< HEAD
Ο δικός σου κώδικας
=======
Ο κώδικας του άλλου branch
>>>>>>> feature-branch

# Επεξεργάσου χειροκίνητα ποιο κομμάτι θες να κρατήσεις, μετά:
git add file.txt
git commit -m "Resolve merge conflict"
```

### ".gitignore" — τι δεν πρέπει ΠΟΤΕ να μπει σε repo
```gitignore
# Secrets/credentials
.env
*.pem
secrets.yaml

# Dependencies (ανακτώνται από package manager)
node_modules/
venv/

# Build artifacts
dist/
*.pyc
__pycache__/

# IDE files
.vscode/
.idea/
```

---

## 🌲 4. Branching Strategies σε Βάθος

### Git Flow
```
main ─────●─────────────●─────────────●──── (μόνο releases)
           \             \             \
develop ────●───●───●─────●───●───●─────●─── (κύρια γραμμή ανάπτυξης)
             \       \
feature/A ────●───●────╯
feature/B ────────●──────────╯
```
- **main**: μόνο σταθερές, released εκδόσεις.
- **develop**: κύρια γραμμή ενσωμάτωσης features.
- **feature/***: κάθε νέο feature, branch από develop.
- **release/***: προετοιμασία release (testing, bug fixes) πριν το merge σε main.
- **hotfix/***: επείγουσες διορθώσεις απευθείας πάνω σε main.

**Πότε ταιριάζει:** Εφαρμογές με προγραμματισμένες, σπάνιες releases (πχ κάθε μήνα).

### GitHub Flow (απλούστερο)
```
main ──●───●───●───●───●───●──── (πάντα deployable)
        \       \       \
feature ─●───●────╯      │
feature ──────────●───●───╯
```
- Μόνο **main** + **feature branches**.
- Κάθε feature branch γίνεται Pull Request → review → merge → deploy.

**Πότε ταιριάζει:** Web εφαρμογές με συνεχή deployment, μικρές ομάδες.

### Trunk-Based Development
```
main ──●●●●●●●●●●●●●●●●●●●●●●●●●●── (όλοι δουλεύουν σχεδόν πάντα εδώ)
```
- Πολύ σύντομα (ή καθόλου) feature branches — commits απευθείας ή σχεδόν απευθείας στο main, πολλές φορές τη μέρα.
- Χρησιμοποιεί **feature flags** για να "κρύβει" ημιτελή χαρακτηριστικά σε production.

**Πότε ταιριάζει:** Ομάδες με ισχυρό automated testing, high-frequency deployment (Google, Facebook style).

---

## ⚙️ 5. GitHub Actions — Πλήρες Pipeline

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run unit tests
        run: npm test

      - name: Build application
        run: npm run build

  docker-build-push:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: contoso/myapp:${{ github.sha }}

  deploy-production:
    needs: docker-build-push
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production   # Απαιτεί manual approval αν έχει οριστεί στο GitHub
    steps:
      - name: Deploy to production
        run: |
          echo "Deploying image contoso/myapp:${{ github.sha }} to production"
          # πχ kubectl set image deployment/myapp myapp=contoso/myapp:${{ github.sha }}
```

### Βασικές έννοιες
| Όρος | Σημασία |
|---|---|
| `on:` | Ποια events ενεργοποιούν το pipeline (push, pull_request, schedule) |
| `jobs:` | Ανεξάρτητες ομάδες βημάτων, μπορούν να τρέξουν παράλληλα |
| `needs:` | Εξάρτηση — αυτό το job περιμένει να τελειώσει άλλο πρώτα |
| `secrets.*` | Ασφαλή, κρυμμένα credentials (βλ. κεφάλαιο 9) |

---

## 🏗️ 6. Jenkins — Πλήρες Pipeline (Jenkinsfile)

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "contoso/myapp"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/contoso/myapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
            post {
                always {
                    junit 'test-results/*.xml'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh "docker login -u $DOCKER_USER -p $DOCKER_PASS"
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Deploy to production?', ok: 'Deploy'
                sh "kubectl set image deployment/myapp myapp=${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }

    post {
        failure {
            mail to: 'devops-team@contoso.com',
                 subject: "Pipeline Failed: ${env.JOB_NAME}",
                 body: "Δες τις λεπτομέρειες: ${env.BUILD_URL}"
        }
    }
}
```

---

## 🦊 7. GitLab CI — Πλήρες Pipeline

```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_IMAGE: "registry.gitlab.com/contoso/myapp"

build:
  stage: build
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/

test:
  stage: test
  script:
    - npm test
  coverage: '/Coverage: \d+\.\d+%/'

deploy_staging:
  stage: deploy
  script:
    - docker build -t $DOCKER_IMAGE:staging .
    - docker push $DOCKER_IMAGE:staging
  environment:
    name: staging
  only:
    - develop

deploy_production:
  stage: deploy
  script:
    - docker build -t $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA .
    - docker push $DOCKER_IMAGE:$CI_COMMIT_SHORT_SHA
  environment:
    name: production
  when: manual          # Χρειάζεται χειροκίνητο "play" button στο GitLab UI
  only:
    - main
```

---

## 🚀 8. Deployment Strategies

Σύνδεση με το `itsm-raci-release-asset-lifecycle.md` που ήδη κάλυψε τα concepts — εδώ το πρακτικό, CI/CD κομμάτι.

| Στρατηγική | Πώς λειτουργεί | Downtime |
|---|---|---|
| **Rolling Update** | Αντικατάσταση instances σταδιακά, ένα-ένα | Μηδενικό (αν σωστά ρυθμισμένο) |
| **Blue-Green** | Δύο πανομοιότυπα περιβάλλοντα, switch traffic ακαριαία | Μηδενικό, άμεσο rollback |
| **Canary** | Νέα έκδοση σε μικρό % traffic πρώτα, σταδιακή αύξηση | Μηδενικό, χαμηλό ρίσκο |
| **Recreate** | Σταμάτημα παλιάς, εκκίνηση νέας | Ναι — απλούστερο αλλά με downtime |

### Παράδειγμα Canary σε Kubernetes (concept)
```yaml
# 90% traffic στην παλιά έκδοση, 10% στη νέα
apiVersion: v1
kind: Service
metadata:
  name: myapp
spec:
  selector:
    app: myapp
---
# Deployment stable (90% των pods)
# Deployment canary (10% των pods)
# Το Service κατανέμει traffic ανάλογα με τον αριθμό pods κάθε deployment
```

---

## 🔐 9. Secrets Management σε Pipelines

### ⚠️ Ποτέ μην κάνεις hardcode credentials στον κώδικα/pipeline
```yaml
# ❌ ΛΑΘΟΣ
- name: Deploy
  run: az login --username admin --password SuperSecret123

# ✅ ΣΩΣΤΟ
- name: Deploy
  run: az login --username ${{ secrets.AZURE_USERNAME }} --password ${{ secrets.AZURE_PASSWORD }}
```

### Πού αποθηκεύονται τα secrets
| Platform | Πού |
|---|---|
| GitHub Actions | Settings → Secrets and variables → Actions |
| GitLab CI | Settings → CI/CD → Variables (mask + protect) |
| Jenkins | Credentials Manager (plugin) |
| Enterprise-grade | HashiCorp Vault, Azure Key Vault, AWS Secrets Manager |

---

## 🔗 10. Πλήρες Παράδειγμα — Από Commit σε Production

```
1. Developer δουλεύει σε feature branch:
   git checkout -b feature/new-login-page
   [...αλλαγές κώδικα...]
   git add . && git commit -m "Add new login page"
   git push origin feature/new-login-page

2. Ανοίγει Pull Request στο GitHub
   → Αυτόματα ενεργοποιείται το CI pipeline (build + test + lint)
   → Απαιτείται τουλάχιστον 1 code review approval (branch protection rule)

3. Μετά από approval, merge στο main
   → Ενεργοποιείται το πλήρες CI/CD pipeline:
      build → test → docker build/push → deploy σε staging

4. Automated acceptance tests τρέχουν στο staging
   → Όλα περνάνε

5. Deployment σε production (με manual approval gate, βλ. "environment: production")
   → IT Manager/Lead εγκρίνει
   → Rolling update deployment, μηδενικό downtime

6. Monitoring (Prometheus/Grafana) επιβεβαιώνει υγιή metrics μετά το deployment
   → Καμία αύξηση error rate

7. Αν κάτι πάει στραβά:
   git revert <commit-hash>
   → Νέο pipeline run, αυτόματο rollback σε προηγούμενη σταθερή έκδοση
```

Αυτή η ροή δείχνει την πλήρη **CI/CD αλυσίδα** — από τον πρώτο κώδικα ενός developer μέχρι το production, με αυτοματοποιημένους ελέγχους σε κάθε βήμα.

---

*Μέρος του [Infrastructure Knowledge Base](https://github.com/Dimitriskatsanos42/Infrastructure-Knowledge-Base) — φάκελος DevOps, συμπληρωματικό στο `devops-fundamentals-roadmap.md`.*
