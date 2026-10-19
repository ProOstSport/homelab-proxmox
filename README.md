# Proxmox-Virtualisierung des Homeservers

> Der Homeserver läuft als Proxmox-VE-Host auf einem Intel NUC. Darin betreibe ich eine Ubuntu-Server-VM für Docker und meine Dienste.

**Status:** abgeschlossen (seit 13.10.2026), in Betrieb

## Ausgangslage

Ubuntu Server lief direkt auf der Hardware. Alle Dienste teilten sich das System.

## Ziel

- Hypervisor und Dienste trennen
- VMs testen, sichern und wiederherstellen können
- Experimente, die das Basissystem nicht beeinträchtigen

## Aufbau

| Komponente | Konfiguration |
|---|---|
| Hypervisor | Proxmox VE 9.2, Hostname `srv-01` |
| Gast | VM 100, Ubuntu Server 26.04.1 LTS, Gast-Hostname `docker-01` |
| VM-Ressourcen | 2 vCPU, 4 GB RAM, 40 GB Systemdisk |
| Netz | VLAN 40 (SRV-01), statische Adressierung |
| Backup-Datenträger | USB-Platte per USB-Passthrough an die VM durchgereicht |

## Entscheidungen

- Proxmox direkt auf der vorhandenen SSD installiert, ohne Aufrüstung von RAM oder Speicher.
- Ubuntu neu installiert statt das bestehende System zu übernehmen. Eine Migration des laufenden Systems war nicht vorgesehen.
- Die USB-Backup-Platte war während der Installation abgesteckt. Das Installationsziel wurde anhand des Modells gewählt, nicht anhand des wechselnden Gerätenamens.

## Lessons Learned

- **Dokumentation zu spät gepflegt:** Alte Planwerte wurden zu lange als aktueller Stand geführt. Ich trenne jetzt klar zwischen Ist-Zustand, Historie und offenen Punkten.
- **Gerätenamen wechseln:** Die USB-Platte erschien mal als `sdb4`, mal als `sdc4`. Eingebunden wird deshalb über die UUID.
- **Backup-Zeiten:** Eine falsche Uhrzeit entstand durch UTC und Verzögerung. Zeitzone und Timer wurden korrigiert.
- **Syntaxprüfung reicht nicht:** Ein Fehler im Backup-Skript (Exit 127) fiel bei `bash -n` nicht auf. Skripte müssen real getestet werden.

## Offene Punkte

- Aktuelle DNS-Konfiguration prüfen
- Proxmox-VM-Backups dokumentieren (letzte Läufe, Restore-Test)
- USB-Stabilität prüfen (Kabel, Port, Stromversorgung)
