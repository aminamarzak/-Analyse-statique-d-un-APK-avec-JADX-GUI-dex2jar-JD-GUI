# -Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI

Application : UnCrackable-Level1.apk
Environnement : Mobexler OS (Linux)
Objectif : Reverse Engineering et Extraction de secrets.
Gemini a dit
Hada rapport kamel m-naddem 3la hsab dakchi li talb l-prof, fih ga3 les Tasks men 1 tal 7. Ktebt lik f kolla blaca chmen Screenshot khassk t-hreg bed-debt b smiytha.

Rapport de TP : Analyse Statique d'Application Android
Application : UnCrackable-Level1.apk

Environnement : Mobexler OS (Linux)

Objectif : Reverse Engineering et Extraction de secrets.

Task 1 — Vérification de l'intégrité
L'analyse commence par le calcul de l'empreinte numérique du fichier APK pour garantir qu'il s'agit du bon binaire.

Commande : sha256sum UnCrackable-Level1.apk

Résultat : 1da8bf57d266109f9a07c01bf7111a1975ce01f190b9d914bcd3ae3dbef96f21
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214253.png?raw=true)
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214351.png?raw=true)


Task 2 — Extraction et Informations de base
Vérification des en-têtes (headers) du fichier pour confirmer qu'il s'agit d'un fichier ZIP/APK (Format PK).
•	Taille du fichier : 66 KB
•	Headers : 4b50 0403 (PK..)
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214319.png?raw=true)
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214408.png?raw=true)
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214336.png?raw=true)



Task 3 — Analyse du Manifeste (JADX GUI)
Utilisation de JADX GUI pour explorer les ressources et la configuration.
•	Package Name : owasp.mstg.uncrackable1
•	Permissions : Aucune permission dangereuse n'est listée, mais le flag android:allowBackup="true" est présent.
•	Activity principale : sg.vantagepoint.uncrackable1.MainActivity.
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214814.png?raw=true)
Task 4 — Recherche de chaînes sensibles
Exploration des fichiers de ressources pour trouver des indices sur le fonctionnement de l'application.
•	Strings.xml : Le fichier contient des labels comme "Enter the Secret String" et "Verify", confirmant la présence d'un challenge de mot de passe.

![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20214956.png?raw=true)
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20221929.png?raw=true)


Analyse du Code Source (Logic de vérification)
L'analyse de la classe MainActivity montre une détection de Root au démarrage et l'appel à une méthode a.a() pour vérifier le secret.
•	Vérification du secret (Critique) : Dans la classe sg.vantagepoint.uncrackable1.a, on trouve une clé AES codée en dur.
•	Clé AES : 8d127684cbc37c17616d806cf50473cc
•	Secret chiffré : 5UJiFctbmgbDoLXmpL12mkno8HT4Lv8dlat8FxR2GOc=
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20215927.png?raw=true)
Task 5 — Conversion DEX vers JAR
Pour une analyse alternative, le fichier classes.dex a été extrait et converti en format .jar pour être lu par d'autres outils comme JD-GUI.
•	Commande utilisée : d2j-dex2jar dex_out/classes.dex
![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20221123.png?raw=true)
Task 6 — Comparaison JADX vs JD-GUI
L'application a été ouverte dans JD-GUI pour comparer les résultats.
Aspect	JADX GUI	JD-GUI
Binya (Structure)	Structure Android complète (Resources + Manifest).	Structure Java uniquement.
Lisibilité	Très bonne, décompile bien le code Android.	Correcte mais manque de contexte Android.
Ressources	Accès direct aux XML et images.	Aucun accès aux ressources.

![Screenshot]( https://github.com/aminamarzak/-Analyse-statique-d-un-APK-avec-JADX-GUI-dex2jar-JD-GUI/blob/main/Screenshot%202026-03-13%20221605.png?raw=true)
Conclusion Finale
L'application UnCrackable Level 1 présente une faille de sécurité majeure : elle stocke ses secrets (Clé AES) directement dans le code source. Un attaquant peut facilement décompiler l'APK et récupérer ces informations pour contourner la sécurité.
Recommandations :
1.	Ne pas stocker de clés de chiffrement en dur (Hardcoded).
2.	Utiliser l'Android Keystore System.
3.	Implémenter une obfuscation de code plus robuste.

