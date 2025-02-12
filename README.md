# 🔥 SQL Injections Tutorial - Pentesting & Wordlists 🔥

## 📌 Introduction
Bienvenue dans le repository **Sql-Injections-Tutorial** ! Ce projet a pour but de fournir des **wordlists SQL Injection** optimisées pour **FFUF, Hydra et BurpSuite** afin d’automatiser les tests d’injection SQL lors d’un pentest web. 

Ce repo couvre également les bases de **la détection des vulnérabilités SQLi** à l’aide d’outils comme **BurpSuite** et **SQLmap**.

---

## 📜 **Contenu du Repository**
- **`Wordlists-Sql-Injections.md`** → Contient des wordlists SQLi classées par niveau :
  - **Wordlist Basique** → Teste les injections SQL classiques.
  - **Wordlist Avancée** → Contourne les filtres WAF et restrictions d’entrée.
  - **Wordlist Blind SQLi** → Exploite les vulnérabilités SQLi en aveugle.
- **Méthodes pour tester une vulnérabilité SQLi** avec BurpSuite et SQLmap.
- **Commandes pour automatiser les attaques SQLi** avec **FFUF, Hydra et BurpSuite**.

---

## 🚀 **Détection d’une Injection SQL**
Avant d’utiliser les wordlists, il est crucial de tester si un champ est vulnérable à une injection SQL. Voici comment faire :

### 🔍 **1️⃣ Tester avec BurpSuite**
1. **Intercepter la requête HTTP** d’un formulaire de login ou d’une requête SQL suspecte.
2. **Envoyer la requête à Intruder** et injecter des payloads SQLi.
3. **Analyser les réponses HTTP** :
   - Une erreur SQL = vulnérabilité possible.
   - Une réponse différente ou un temps de chargement anormal = exploitation possible via Blind SQLi.

### 💀 **2️⃣ Tester avec SQLmap**
SQLmap permet de tester et d’exploiter automatiquement une injection SQL :
```bash
sqlmap -u "http://target.com/login.php?username=admin&password=test" --batch --dbs
```
📌 **Si la requête est vulnérable, SQLmap révélera les bases de données disponibles.**

---

## 🛠 **Utilisation des Wordlists**
### 🚀 **1️⃣ Utilisation avec FFUF**
```bash
ffuf -u "http://target.com/login.php?username=FUZZ&password=test" -w sql_injection_wordlist.txt
```
📌 **FFUF enverra les payloads SQLi dans le paramètre `username` et analysera les réponses.**

### 🚀 **2️⃣ Utilisation avec Hydra**
```bash
hydra -L users.txt -P sql_injection_wordlist.txt target.com http-post-form "/login.php:username=^USER^&password=^PASS^:Login failed"
```
📌 **Hydra teste les injections SQL pour contourner l’authentification.**

### 🚀 **3️⃣ Utilisation avec BurpSuite Intruder**
📌 **Étapes :**
1. Intercepter la requête avec **BurpSuite Proxy**.
2. Envoyer la requête à **Intruder**.
3. Injecter les payloads SQLi dans le champ vulnérable.
4. Lancer l’attaque et analyser les réponses HTTP.

---

## 🎯 **Objectif du Repository**
Ce projet est destiné aux **pentesters et étudiants en cybersécurité** qui veulent :
- Apprendre **comment détecter une injection SQL** efficacement.
- Automatiser les attaques SQLi avec **FFUF, Hydra et BurpSuite**.
- Utiliser **SQLmap pour tester et exploiter une injection SQL**.

✅ **Forkez ce repo, testez les wordlists et contribuez !** 🚀🔥

---

## 📚 **Ressources utiles**
- [OWASP SQL Injection Guide](https://owasp.org/www-community/attacks/SQL_Injection)
- [SQLmap Documentation](https://sqlmap.org/)
- [BurpSuite Intruder Tutorial](https://portswigger.net/burp/documentation/desktop/tools/intruder)

💀 **Happy Hacking!** 🚀🔥
