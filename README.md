# IoT Sensor Project – Kävijämäärien seuranta ja ennustaminen

**Tekijät:** Oskari Luolila, Toivo Pirhonen, Robin Say  
**Kurssi:** Data ja koneoppiminen – Metropolia AMK, kevät 2026

---

## Projektin kuvaus

Tässä projektissa rakensimme automaattisen IoT-datapipelinen, joka kerää kävijämääräanturidataa Myyrmäen kampuksen tiloista, tallentaa sen pilvipalveluun ja analysoi sen koneoppimismenetelmin.

Järjestelmä koostuu seuraavista osista:
- **Datankeruu:** MQTT-protokolla → Node-RED → MongoDB Atlas
- **Pilvipalvelu:** Node-RED hostattu Railway-palvelussa (24/7)
- **Dashboard:** Reaaliaikainen visualisointi Node-RED Dashboardilla
- **Analyysi:** Google Colab + Python (Prophet, ARIMA, aikasarja-analyysi)

---

## Repositorion rakenne

```
iot-sensor-project/
│
├── flows.json              # Node-RED flow – datankeruu ja dashboard
├── analysis.ipynb          # Google Colab notebook – data-analyysi ja ennustemallit
├── loppuraportti.docx      # Projektin loppuraportti
└── README.md               # Tämä tiedosto
```

---

## Pipeline

```
IR-anturi (ESP32)
      ↓ MQTT
Node-RED (Railway)
      ↓
MongoDB Atlas
      ↓
Google Colab (analyysi)
```

---

## Analyysimenetelmät

| Menetelmä | Kuvaus |
|-----------|--------|
| Aikasarjan hajotus | Trendi, kausivaihtelu ja residuaali |
| Tuntikeskiarvot | Vilkkaimmat kellonajat |
| Viikonpäivävertailu | AIoT Garage vs Robotiikkahuone |
| Box plot | Kävijämäärien jakauma tunneittain |
| Prophet | Facebook-kehittämä ennustemalli |
| ARIMA | Klassinen tilastollinen aikasarjamalli |

---

## Ennustemallien tulokset

| Malli | Tila | MAE | RMSE |
|-------|------|-----|------|
| Prophet | AIoT Garage | 0.50 | 0.78 |
| Prophet | Robotiikkahuone | 0.39 | 0.61 |
| ARIMA | AIoT Garage | 0.28 | 0.48 |
| ARIMA | Robotiikkahuone | 0.33 | 0.78 |

**ARIMA** oli tarkempi lyhyen aikavälin ennustuksissa (MAE), **Prophet** parempi kausivaihtelun analyysissa.

---

## Teknologiat

- **Node-RED** – datankeruu ja dashboard
- **MongoDB Atlas** – pilvipalvelu tietokannalle
- **Railway** – Node-REDin hosting
- **Python** – pandas, prophet, statsmodels, matplotlib, scikit-learn
- **Google Colab** – analyysiympäristö

---

## Dashboard

Reaaliaikainen dashboard pyörii osoitteessa:  
🔗 https://node-red-production-3309.up.railway.app/dashboard

---

## Keskeiset löydökset

- Molemmissa tiloissa selkeä päivittäinen rytmi — vilkkain aika **klo 11–12**
- AIoT Garage vilkkain **maanantaisin**, Robotiikkahuone **tiistaisin ja keskiviikkoisin**
- Viikonloppuisin tilat käytännössä tyhjiä
- ARIMA-malli saavutti erinomaisen tarkkuuden: **MAE 0.28 kävijää**
