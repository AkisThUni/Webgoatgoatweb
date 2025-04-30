# 📄 Vulnerability Report - WebGoat

## 🔍 Εντοπισμένα Alerts

| Severity | Είδος Ευπάθειας     | Αρχείο                          | Περιγραφή                                      | Link στο CVE |
|----------|---------------------|----------------------------------|------------------------------------------------|----------------|
| Critical | [1] SQL Injection       | webgoat-lessons/SQLInjection.java | Εισαγωγή μη φιλτραρισμένων εισόδων στον SQL query. | [Alert](#)     |
| Critical | [2] Command Injection   | webgoat-lessons/Exec.java         | Εκτέλεση εντολών κελύφους με μη φιλτραρισμένα δεδομένα. | [Alert](#)     |
| High     | [3] Path Traversal      | webgoat-lessons/FileAccess.java   | Επιτρέπει πρόσβαση σε αρχεία εκτός του επιτρεπόμενου path. | [Alert](#)     |
| High     | [4] Cross-Site Scripting (XSS) | webgoat-lessons/XSS.java          | Μη κωδικοποιημένη έξοδος HTML επιτρέπει injection script. | [Alert](#)     |
| High     | [5] Hardcoded Credentials | webgoat-lessons/Auth.java        | Χρήση σταθερών διαπιστευτηρίων στον κώδικα.     | [Alert](#)     |

---

## 🛡️ Προτεινόμενα Μέτρα Αντιμετώπισης

### [1] SQL Injection
- Χρήση Prepared Statements αντί για δυναμικά queries.
- Αποφυγή εισαγωγής user input χωρίς έλεγχο τύπου/μορφής.

### [2] Command Injection
- Χρήση `ProcessBuilder` με αυστηρό έλεγχο ορίων εισόδου.
- Απομόνωση λειτουργιών κελύφους σε ασφαλές sandbox.

### [3] Path Traversal
- Κανονικοποίηση διαδρομών με `Paths.get(...).normalize()`.
- Περιορισμός προσβάσιμων καταλόγων με λευκή λίστα.

### [4] Cross-Site Scripting (XSS)
- Κωδικοποίηση χαρακτήρων στην HTML έξοδο (π.χ. με Apache Commons StringEscapeUtils).
- Χρήση HTTP headers για περιορισμό scripting (Content-Security-Policy).

### [5] Hardcoded Credentials
- Μεταφορά credentials σε περιβαλλοντικές μεταβλητές ή config αρχεία εκτός VCS.
- Χρήση vaults ή secrets management εργαλείων (π.χ. GitHub Secrets).

---

## 🔁 Κατάσταση μετά τη Διόρθωση

| Ευπάθεια | Κατάσταση | Σχόλιο |
|----------|-----------|--------|
| [1] SQL Injection | ✅ Fixed | Έγινε χρήση PreparedStatement. |
| [2] Command Injection | ✅ Fixed | Αφαιρέθηκε η χρήση `Runtime.exec`. |
| [3] Path Traversal | ✅ Fixed | Προστέθηκε έλεγχος με canonical path. |
| [4] XSS | ✅ Fixed | Εφαρμόστηκε HTML encoding. |
| [5] Hardcoded Credentials | ✅ Fixed | Μεταφέρθηκαν σε αρχείο `.env`. |

---
