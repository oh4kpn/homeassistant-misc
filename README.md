# homeassistant-misc

# dallas geneerinen kuuntelija hirviö:


# 🌡️ Kellarin OneWire-Hirviö (v1.1) - dallas-kuuntelija.yaml

Tämä projekti on dedikoitu ESP32-pohjaiselle OneWire-hubille, joka hallitsee useampaa fyysistä väylää ja yhteensä useampia DS18B20-lämpötila-anturia. Projekti on suunniteltu Hämylän varaajahuoneen ja kellaritilojen tinkimättömään valvontaan.

## 🛠️ Arkkitehtuuri

Laite käyttää ESP-IDF-frameworkia ja ESPHome-alustaa. Toisin kuin perinteiset sensoritoteutukset, tämä hubi lähettää jokaisen mittauksen reaaliaikaisena tapahtumana suoraan Home Assistantin **Event Bus iin**. Tämä mahdollistaa dynaamisen datan käsittelyn, viiveanalyysin ja laajan skaalautuvuuden ilman kiinteitä entiteettilukituksia - kuten esimerkiksi että esp32:n vikaantuessa pitäisi kaivella backupeista kyseisen esp32:n sensor konffia.

### Esimerkki Väyläjako:
* **Väylä 1 (GPIO25):** 4 anturia
* **Väylä 2 (GPIO26):** 4 anturia
* **Väylä 3 (GPIO18):** 2 anturia

## 🧠 Toteutustapa: Explicit Copypasta Architecture™

Koska `dallas/one_wire` -komponentin tuki automaattiselle skannaukselle ja iteraatiolle on nykyisissä versioissa rajallinen, toteutus on tehty eksplisiittisesti määrittelemällä jokainen anturi erikseen. Tämä takaa:
1. **Maksimaalisen kontrollin**: Jokaisella anturilla on omat suodattimensa.
2. **Diagnostiikan**: Virheelliset arvot (`85.0`, `-127.0`, `0.0`, `nan`) karsitaan jo laitteen päässä.
3. **Pomminkestävyyden**: Väylähäiriöt yhdellä linjalla eivät vaikuta muihin.

## 📡 Datan muoto

Laite lähettää `esphome.dallas_raw` -tapahtumia, jotka sisältävät täydellisen aikaleiman aikavyöhyketiedolla. Tämä varmistaa datan eheyden, vaikka viestinvälityksessä tapahtuisi viivettä.

### Esimerkki JSON-paketista:
```json
{
    "event_type": "esphome.dallas_raw",
    "data": {
        "address": "0x9c000000847d4828",
        "value": "18.31",
        "hub": "kellari-onewire-hub-1",
        "measured_at": "2026-02-07T20:35:12+0200"
    }
}
