# 🚄 MyTreno  
![logo](https://github.com/lotablet/mytreno/blob/main/custom_components/mytreno/images/logo.png)

> Integrazione Home Assistant per monitorare in tempo reale **partenze e arrivi dei treni italiani** tramite il servizio **ViaggiaTreno** di Trenitalia.

---

**MyTreno** ti permette di tenere d'occhio i treni della tua stazione preferita, direttamente da Lovelace.  
Perfetto se vivi vicino a una stazione, se aspetti qualcuno o se vuoi farti i cazzi della Trenitalia per vedere quanti ritardi ci sono 😄

---

## ✅ Funzionalità attuali

- Recupero automatico delle **partenze** e degli **arrivi** da qualsiasi stazione italiana (basta conoscere il codice `SXXXXX`)
- Integrazione configurabile **via UI** da *Impostazioni → Integrazioni*
- Sensore aggiornato in tempo reale con treni in transito
- Card Lovelace pronta all’uso (per ora in versione `config-template-card`, ma in arrivo quella ufficiale da **HACS**)

---

## 📦 Installazione tramite HACS

1. Vai in **HACS → Integrazioni**
2. Clicca su **"..." → Aggiungi repository personalizzato**
3. Inserisci questo URL e scegli tipo **"Integrazione"** :
```
https://github.com/lotablet/mytreno
```


4. Dopo aver aggiunto il repository, cerca “**MyTreno**” nell’elenco delle integrazioni HACS
5. Installa e riavvia Home Assistant

### 🔧 Configurazione

1. Vai su **Impostazioni → Dispositivi e Servizi**
2. Clicca su **"Aggiungi Integrazione"**
3. Cerca **MyTreno** e inserisci la città.

---

## 💡 Card Lovelace (provvisoria)

Questa è una card già funzionante, che puoi copiare direttamente in Lovelace:

1. **Sostituisci le entità** con quelle generate dalla tua integrazione (`CTRL + F` per fare replace al volo)
2. Cambia il nome della città nel primo blocco di testo (`markdown`)
3. Goditi l’orario ferroviario direttamente in dashboard

> La versione ufficiale della card Lovelace sarà disponibile a breve direttamente da HACS 😉

---

⚠️ Il repository è ancora in fase di aggiornamento, ma la base è solida e già pronta all’uso.  


```
type: custom:config-template-card
entities:
  - sensor.mytreno_s06000
card:
  type: markdown
  content: >
    La Spezia Centrale
    
    ---
    
    ## 🚆 Partenze

    {% for treno in state_attr('sensor.mytreno_s06000', 'partenze')[:5] %} 🔹
    **{{ treno.treno }}** → {{ treno.destinazione }}   🕒 {{ treno.orario }}  
    {% if treno.ritardo > 5 %} ⏱️ **Ritardo +{{ treno.ritardo }} min** 🔴 {%
    elif treno.ritardo > 0 %} ⏱️ Ritardo +{{ treno.ritardo }} min 🟡 {% elif
    treno.ritardo < 0 %} 🟢 Anticipo {{ treno.ritardo | abs }} min {% else %} ✅
    In orario {% endif %}

    {% endfor %}

    ---

    ## 🛬 Arrivi

    {% for treno in state_attr('sensor.mytreno_s06000', 'arrivi')[:5] %} 🔹 **{{
    treno.treno }}** ← {{ treno.provenienza }}   🕒 {{ treno.orario }}   {% if
    treno.ritardo > 5 %} ⏱️ **Ritardo +{{ treno.ritardo }} min** 🔴 {% elif
    treno.ritardo > 0 %} ⏱️ Ritardo +{{ treno.ritardo }} min 🟡 {% elif
    treno.ritardo < 0 %} 🟢 Anticipo {{ treno.ritardo | abs }} min {% else %} ✅
    In orario {% endif %}

    {% endfor %}
```
