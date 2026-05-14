# 🔄 1.HANDOVER – MeshCore Responder

Stand: 13.05.2026

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

# 🔄 2. HANDOVER – MeshCore Responder

Aktualisierung: 14.05.2026

---

## 🎯 Projektziel

Automatischer Antwort-Bot fuer MeshCore-Direktnachrichten.
Empfaengt DMs, antwortet mit technischen Infos (RSSI, SNR, Pfad)
und dokumentiert alles in CSV-Dateien.

---

## Status: Prototyp funktioniert!

- Radio: Heltec V3, Firmware v1.15.0-dee3e26
- Name im Netz: PAUL3400 Responder
- Public Key: 6b0b1df2...4b76c874
- Preset: EU/UK (Narrow), Switzerland, 869.618 MHz
- Laeuft auf: RpPi4B-001
- Port: /dev/ttyUSB0 (CP2102 Chip)
- Python-Bibliothek: meshcore 2.3.7
- Startbefehl: python3 ~/test_listen.py

---

## Erkenntnisse API (meshcore 2.3.7)

- Verbindung: MeshCore.create_serial("/dev/ttyUSB0")
- Device-Info: mc.commands.send_device_query()
- Kontakte laden: mc.commands.get_contacts()
- Advert senden: mc.commands.send_advert(flood=True)
- Nachricht senden: mc.commands.send_msg(contact, text)
- Kontakt finden: mc.get_contact_by_key_prefix(key)
- Message-Polling: mc.start_auto_message_fetching()

EventTypes:
- CONTACT_MSG_RECV: DM empfangen (hat SNR, text, pubkey_prefix)
- RX_LOG_DATA: Rohdaten (hat rssi, path_len, path)
- RSSI fehlt in CONTACT_MSG_RECV, muss aus RX_LOG_DATA geholt werden
- path_len in CONTACT_MSG_RECV ist 255 (ungueltig), aus RX_LOG_DATA nutzen

Pfad-Logik:
- path_len == 0 und path == '' -> "Direkt"
- path_len > 0 -> path enthält 2-byte Hashes der Repeater (z.B. "231e")

---

## Antwort-Format (max 110 Zeichen)

Beispiel direkt: Responder|SNR:12.25 RSSI:-42|Direkt
Beispiel Repeater: Responder|SNR:8.5 RSSI:-87|231e>4f0a

---

## Erledigte Schritte

- [x] Repo erstellt auf GitHub
- [x] Radio geflasht (MeshCore Client, v1.15.0)
- [x] Radio an RpPi4B-001 angeschlossen (USB)
- [x] Serieller Port identifiziert (/dev/ttyUSB0, CP2102)
- [x] meshcore 2.3.7 installiert
- [x] Verbindungstest erfolgreich
- [x] Advert Flood funktioniert (Node sichtbar im Netz)
- [x] DM-Empfang funktioniert (EventType.CONTACT_MSG_RECV)
- [x] Automatische Antwort funktioniert (send_msg)
- [x] Pfad-Erkennung: Direkt vs. Repeater-Hashes

---

## Naechste Schritte

- [ ] CSV-Logging einbauen (csv_logger.py)
- [ ] Individuellen Text in Antwort einbauen (~50 Zeichen)
- [ ] Repeater-Test (Nachricht via Seedtest senden)
- [ ] Code von test_listen.py nach responder.py verschieben
- [ ] Fehlerbehandlung (Verbindungsabbruch, Reconnect)
- [ ] systemd-Service einrichten
- [ ] Code auf GitHub pushen

---

## Korrektur

- Companion-Node am iPhone heisst PAUL3400 (nicht Paul3600)
