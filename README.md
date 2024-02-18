`- Version: 1.0 -` 
### **Supportaci**
Se apprezzi questo progetto, ci piacerebbe avere il tuo supporto, anche con un semplice caffè. 
I fondi raccolti saranno utilizzati per acquistare nuovo materiale e realizzare nuovi progetti. Puoi contribuire cliccando sul pulsante qui sotto. 
Grazie di cuore per il tuo sostegno!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/M4M1MI00I)

# Gestione valvole termostatiche TADO
Progetto originariamente neto per la gestione delle riscaldamento con Netatmo e successivamente modificato per Tado.
<img width="1148" alt="generale" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/085b1c7c-2761-4e3e-97a7-48c3023011ac">

![finestra_info](https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/0f75bd03-eee8-4a95-bdc7-f45f3239323a)
![ricambio_aria](https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/777fbdbb-c41a-4251-aec1-873dd2657cf9)

![setting_info](https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/50d91c58-2726-470a-8615-275f897da8ec)

## Prerequisiti:
- File history_stat_custom.jinja e history_stat.jinja presente nella cartella custom_templates. (Se non c'è, create la cartella custom_templates nella root principale di HA e poi copiate il file history_stat_custom.jinja. Riavviate HA.
- Ogni valvola dovrà essere chiamata climate.valvola_NOMEDELLASTANZA es climate.valvola_soggiorno
- Gruppo famiglia esistente
- Browser_mod
- Il servizo di notifiche è affidato al fantastico Multinotify di Henrik vi consiglio di leggere questo articolo https://henriksozzi.it/2022/01/package-multinotify-notifiche-su-alexa-e-app/ oppure di sostituire le notifiche con un vostro servizio
- custom:apexcharts-card
- Custom Button Card installato con la gestione dei template. Doc di come fare qui: https://github.com/custom-cards/button-card#configuration-templates
in Generale, basterebbe creare nella root principale la cartella "buttom_templates" dove metterci all'interno il file template.yaml e mettere questa srtinga all'inizio del file lovelace button_card_templates: !include_dir_merge_named buttom_templates/
- Gestione dei packages in HA. Doc ufficiale https://www.home-assistant.io/docs/configuration/packages/

## Descrizine
Questo package prefigge l'obbiettivo di gestire e ottimizzare il riscaldamento di casa gestendo anche l'apertura delle finestre e il ricambio d'aria.

### Ricambio Aria ![ricambio aria icona](https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/d00890f4-3f27-4cfa-8757-69992c0cbbdf)
Questa icona (normalmente bianca) si colorerà man mano con toni sempre più gravi per indicare il bisogno di un ricambio d'aria sempre maggiormente necessario.
Il sistema si basa solo su un calcolo temporale: più passa il tmepo senza che nessuna finestra nella stanza venga aperta, e più il ricambio d'aria diventa grave. Cliccando sull'icona, si può avere il dettagli di quanto è urgente il ricambio d'aria e di quante ore sono passate dall'ultima volta.

### Sensore Finestra  
indica se la finestra è chiusa <img width="64" alt="finestra_chiusa" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/b362baf9-aa4e-4a46-96b4-0aa1d7e32ea1"> o da quanto è stata aperta  <img width="64" alt="finestra_aperta" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/4c252646-88d0-4eac-8d0e-617e447ba902">

Cliccando sull'icona si ottiene il dettaglio dell'ultima apertura e del numero di aperture totali della giornata
Dopo 3 minuti dall'apertura della finestra, il sensore di ricambio d'aria viene azzerato e il contatore delle aperture viene incrementato.
Quando la finestra viene aperta, dopo un tempo prestabilito la valvola di quella stanza viene chiusa per evitare sprechi. Sempre con un tempo prestavilito, parte anche una notifica che ricorda che la finistra è aperta. Si può impostare anche un altre timer ancora al termine del quale parte anche un avviso con Alexa.

### Impostazioni <img width="37" alt="setting" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/5aef2140-5212-49a5-b365-e59dda53e963">
Nella finestra impostazionio abbiamo invece nell'ordine:
- ON/OFF della gestione generale della singola valvola.
- ON/OFF della ricezione della notifica che avvisa che la finestra della stanza è rimasta aperta.
- Impostazione del tempo che deve trascorrere prima che parta la suddetta norifica venga inviata.
- ON/OFF della notifica vocale per la finestra lasciata aperta.
- Impostazione del tempo che deve trascorrere prima che parta la suddetta norifica venga azionata.
- ON/OFF dell'avviso TrueTemperature: un controllo attraverso il quale riceveremo una notifica se la temperatura della stanza rilevata dalla valvola è troppo diversa da un altro sensore di temperatura sempre nella stessa staza. Utile per scopriore se c'è bisogno di una taratura della valvola con un offset manuale.
- ON/OFF della notifica che avvisa di una eventuale disconnessione della valvola.
- Onn/OFF della notidfica relativa alle batterie scariche.

## Card Valvola

<img width="415" alt="valvole" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/b5a75c35-102d-45ae-b07f-f591b1d15930">

Nel cerchio grade è indicata la temperatura attuale della stanza, il tasso di umidità relatica. Ne quadrato sopra invece è indicata la temperatura target.
E' possibile intervenire manualmente sulla temperatura con + e - oppure spegnere la valvola con il tastino in alto a sinistra.
Per quanto rigurada le statistiche credo che non ci siano bisogno di spiegazioni, mentre il grafico indica con il rosso la temperatura della stanza, con il nero la temperatura esterna e con le colonne l'apertura della valvola. 

Quando la valvola è chiusa si colora di nero, nel casso fosse disconnessa diventa grigia altrimenti bianca. Quando invece è in funzione, viene aggiunto uno sfondo inconfondibile più l'icona della fiamma.

## Pannllo generale 


In questo pannello abbiamo:
<img width="384" alt="pannello" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/38676c13-1bbd-4983-a12b-f66d91982c4f">


- il conteggio in tempo reale di quante valvole sono accese.
- Il totale delle ore giornaliere di funzionamente dell'impianto.
- Tasto per impostare la modalità fuori casa.<img width="45" alt="away" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/84e388d9-190b-430c-9578-253a9c6fdfd5">

- Tasto per impostare la modalità in casa. <img width="45" alt="home" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/f04cb301-31a8-41be-af15-bde69b8d2ee5">

- Tasto per spengere tutte le valvole.<img width="45" alt="off" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/a5fb7d96-e646-46a7-a6c8-805e14a7d330">

Con questi 3 tasti invece abbiamo rispetticamente:
<img width="199" alt="button_pannello" src="https://github.com/Home-Assistant-Pro-Team/Gestione-valvole-TADO/assets/48358142/9ff16435-83de-49f0-8b5c-85a55d5a296d">

- Accensione al rientro, al rientreo del gruppo famiglia, le impostazioni delle valvole torneranno a HOME (in casa)
- Se nessuno è in casa, e il riscaldamento è acceso, appena una valvola farà partire la caldaia partirà un alert actionable con la richiestea di spegnere o lasciare acceso il riscaldamento. Questa notifica si ripterà ogni 5 minuti all'infinito finquando non verrà data una rimposta.
- L'utimo tasto accene o spenge la gestione generale del ricambio d'aria.


# Setup
dopo aver soddisfattto i prerequisiti,
- Copiate i file package nella vostra cartella di destinazine dei packages.
- Copiate il file template della vostra destinazione per i template dei custom button card
- Copiate le card replicandole quante volte volete.

Tutti i file di esempio sono redatti sulla mia camera studio, di conseguenza, vi baseterà in ogni file cercare la parola "studio" per trovare tutte le righe da editare con le vostre entità.

### **Supportaci**
Se apprezzi questo progetto, ci piacerebbe avere il tuo supporto, anche con un semplice caffè. 
I fondi raccolti saranno utilizzati per acquistare nuovo materiale e realizzare nuovi progetti. Puoi contribuire cliccando sul pulsante qui sotto. 
Grazie di cuore per il tuo sostegno!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/M4M1MI00I)
