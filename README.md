# conny-memory
# TravelTide Rewards: Kundensegmentierung & Perk-Zuordnung

## Projektüberblick

**TravelTide** ist eine digitale Plattform zur Buchung internationaler Flüge und Hotels. Seit der Gründung 2021 verzeichnet das Unternehmen stetiges Wachstum. Im Rahmen dieser Analyse wurde ein datengestütztes Segmentierungsmodell erstellt, um ein geplantes Kundenbindungsprogramm optimal umzusetzen.

**Projektbeteiligte:**
- Elena, Head of Marketing bei TravelTide
- Cornelia Brinkmann, Masterschool

## Zielsetzung

Ziel war es, Nutzergruppen anhand ihres Buchungsverhaltens zu identifizieren und jedem Kundensegment einen passenden „Reward“ (Perk) zuzuweisen. Diese Belohnungen sollen gezielt die Wahrscheinlichkeit von Wiederbuchungen steigern. Zur Auswahl standen folgende Perks:

- Free hotel meal
- Free checked bag
- No cancellation fees
- Exclusive discounts
- 1 night free hotel with flight

Nur Nutzer mit mehr als sieben Sessions nach dem 04.01.2023 wurden in die Analyse aufgenommen.

## Explorative Datenanalyse (EDA)
Die Grundlage bildeten vier Tabellen:
- **sessions**: 5,4 Mio. Zeilen, Infos zu Nutzerinteraktionen
- **flights**: 1,9 Mio. Zeilen, Details zu Flugbuchungen
- **hotels**: 1,9 Mio. Zeilen, Infos zu Hotelbuchungen
- **users**: 1 Mio. Zeilen, demografische Angaben

**Datenprobleme & Besonderheiten:**
- **Ungültige Hotelnächte:** `check_in_time` lag teilweise nach `check_out_time`. Wurde korrigiert.
- **Stornierungen:** Jede Trip-ID konnte doppelt vorkommen (gebucht & storniert). Lösung: Nur tatsächliche Stornos wurden mit einer `MAX()`-Logik identifiziert.
- **Unvollständige Daten:** Nur Flugbuchungen mit und ohne Hotel waren storniert – reine Hotelstornos ergab das Resultat 0.
- **Hohe Reisedistanzen:** Viele Nutzer reisten über 8000 km. Nur Nutzer mit Flugdaten konnten Distanzmetriken erhalten.

## Datenmodellierung

Alle Tabellen wurden über `trip_id` oder `user_id` zusammengeführt. Danach wurden:
- Sessions bereinigt
- Buchungsdauer und Buchungsverhalten analysiert
- Demografische Merkmale (Alter, Familienstand, Kinder) integriert
- Metriken auf Trip- und Session-Ebene berechnet

Wichtige berechnete Merkmale waren u. a.:
- durchschnittliche Distanz (Haversine-Methode)
- Stornoquote
- Buchungshäufigkeit
- Hotel- und Flugkosten
- durchschnittliche Page Clicks pro Session
- durchschnittliche Tage bis zum Reisebeginn

## Segmentierung

Basierend auf Metriken und Perzentilen wurden 5782 Nutzer in folgende Gruppen unterteilt:

| Segment                | Beschreibung                                                                 |
|------------------------|------------------------------------------------------------------------------|
| **Business**           | Vielflieger, meist Alleinreisend, wenig Storno                               |
| **Long-Distance**      | Internationale Reisen, wenig Extras                                          |
| **Bargain-Shopper**    | Rabattfokussiert, viele Stornos                                              |
| **Luxury**             | Teure Hotels, hohe Ausgaben                                                  |
| **Last-minute-Booker** | Kurzfristige Buchungen, oft nur Hotel                                        |
| **Hesitant**           | Viele Klicks, kaum oder keine Buchungen                                      |
| **Other**              | Nutzer, die keinem der obigen Muster eindeutig zugeordnet werden konnten     |

## Perk-Zuordnung

Jedes Segment erhielt einen individuellen Vorteil:

| Segment                | Zugewiesener Perk                         |
|------------------------|-------------------------------------------|
| Business               | 1 night free hotel with flight            |
| Long-Distance          | Free checked bag                          |
| Bargain-Shopper        | No cancellation fees                      |
| Last-minute-Booker     | Exclusive discounts                       |
| Luxury                 | Free hotel meal                           |
| Hesitant               | Exclusive discounts                       |

## Empfehlung

Ein Rollout über personalisierte E-Mail-Kampagnen wird empfohlen, kombiniert mit A/B-Tests zur Messung der Conversion Rate. Die Integration in Tableau und CRM-Systeme wurde vorbereitet.

---
