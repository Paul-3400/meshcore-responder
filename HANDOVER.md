# 🔄 HANDOVER – MeshCore Responder

Letzte Aktualisierung: 13.05.2026

---

## 🎯 Projektziel

Automatischer Antwort-Bot für MeshCore-Direktnachrichten (DMs).
Empfängt Nachrichten, antwortet mit technischen Infos zum
Übertragungsweg und dokumentiert alles in CSV-Dateien.

---

## ✅ Entscheidungen (13.05.2026)

- Hardware: RpPi4B-001 (neben Dashboard)
- Radio: Separates Heltec V3, eigener Knoten, via USB
- Max. Antwortlänge: 110 Zeichen gesamt
- Davon individueller Text: ~50 Zeichen
- Davon technische Infos: ~60 Zeichen
- Logging: CSV (Excel/Numbers-kompatibel)
- Service: systemd
- Python-Bibliothek: meshcore

---

## 🏗️ Architektur

- RpPi4B-001 ← USB/Serial → Heltec V3 (eigener Node)
- responder.py lauscht auf DMs via meshcore-Bibliothek
- Aktivitäten werden in logs/activity_log.csv geschrieben

---

## 📡 Antwort-Format (max. 110 Zeichen)

Beispiel:
📡 Paul-Responder ✅ | RSSI:-87 SNR:7.5 Hops:2 | Deine Msg empfangen!

Aufbau:
- Prefix/Name: ~15 Zeichen
- Technische Daten: ~45 Zeichen
- Individueller Text: ~50 Zeichen

---

## 📊 CSV-Spalten

timestamp, sender_key, sender_name, message_in, rssi, snr,
hops, path, response_sent, response_status

---

## 🚧 Nächste Schritte (Phase 1)

1. [ ] Repo auf GitHub erstellt
2. [ ] Radio flashen (MeshCore Client-Firmware)
3. [ ] Radio an RpPi4B-001 via USB anschliessen
4. [ ] Seriellen Port identifizieren
5. [ ] pip install meshcore auf RpPi4B-001
6. [ ] Basis-Skript: Verbindung herstellen, auf DMs lauschen

---

## 🔗 Verwandte Projekte

- [observer](https://github.com/Paul-3400/meshcore-duty-cycle-observer)
- [dashboard](https://github.com/Paul-3400/meshcore-duty-cycle-dashboard)
- [standort-evaluator](https://github.com/Paul-3400/meshcore-standort-evaluator)

---

## 📚 Ressourcen

- Docs: https://docs.meshcore.io/
- Blog: https://blog.meshcore.io/
- Flasher: https://flasher.meshcore.io/
