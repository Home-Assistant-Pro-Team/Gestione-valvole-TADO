`- Version: 1.0 -` 
### **Supportaci**
Se apprezzi questo progetto, ci piacerebbe avere il tuo supporto, anche con un semplice caffè. 
I fondi raccolti saranno utilizzati per acquistare nuovo materiale e realizzare nuovi progetti. Puoi contribuire cliccando sul pulsante qui sotto. 
Grazie di cuore per il tuo sostegno!

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/M4M1MI00I)

# Gestione valvole termostatiche TADO
Progetto originariamente neto per la gestione delle riscaldamento con Netatmo e successivamente modificato per Tado.
![405882255_6631155697012498_3849374981296037721_n](https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/f016d1ae-c334-41bb-ac17-3fc491146afe)
![Screenshot 2023-12-01 164128](https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/09b1f49e-8c0b-4a9e-9cb9-f29475c38903)



![Screenshot 2023-12-01 164205](https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/91c68318-52ac-4a93-baa9-e5166d78e72b)
![Screenshot 2023-12-01 164341](https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/1123dd4e-8c03-46b8-a93f-fc669d3a4a82)

## Prerequisiti:
- File history_stat_custom.jinja presente nella cartella custom_templates. (Se non c'è, create la cartella custom_templates nella root principale di HA e poi copiate il file history_stat_custom.jinja. Riavviate HA.
- Ogni valvola dovrà essere chiamata climate.valvola_NOMEDELLASTANZA es climate.valvola_soggiorno
- Gruppo famiglia esistente
- Browser_mod
- custom:apexcharts-card
- Custom Button Card installato con la gestione dei template. Doc di come fare qui: https://github.com/custom-cards/button-card#configuration-templates
in Generale, basterebbe creare nella root principale la cartella "buttom_templates" dove metterci all'interno il file template.yaml e mettere questa srtinga all'inizio del file lovelace button_card_templates: !include_dir_merge_named buttom_templates/
- Gestione dei packages in HA. Doc ufficiale https://www.home-assistant.io/docs/configuration/packages/

## Descrizine
Questo package prefigge l'obbiettivo di gestire e ottimizzare il riscaldamento di casa gestendo anche l'apertura delle finestre e il ricambio d'aria.

### Ricambio Aria 
![Screenshot 2023-12-01 165044](https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/bf285af5-099d-4d1c-b1e9-d4034f850883) Questa icona (normalmente bianca) si colorerà man mano con toni sempre più gravi per indicare il bisogno di un ricambio d'aria sempre maggiormente necessario.
Il sistema si basa solo su un calcolo temporale: più passa il tmepo senza che nessuna finestra nella stanza venga aperta, e più il ricambio d'aria diventa grave. Cliccando sull'icona, si può avere il dettagli di quanto è urgente il ricambio d'aria e di quante ore sono passate dall'ultima volta.

### Sensore Finestra  
indica se la finestra è chiusa <img width="64" alt="Screenshot 2023-12-02 alle 10 00 08" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/8bc2f88d-e804-4e71-a426-e6235f7b0a36"> o da quanto è stata aperta <img width="64" alt="Screenshot 2023-12-02 alle 10 00 01" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/c1e24a4a-fd69-40cc-8abb-da65eebecf11"> 
Cliccando sull'icona si ottiene il dettaglio dell'ultima apertura e del numero di aperture totali della giornata
Dopo 3 minuti dall'apertura della finestra, il sensore di ricambio d'aria viene azzerato e il contatore delle aperture viene incrementato.
Quando la finestra viene aperta, dopo un tempo prestabilito la valvola di quella stanza viene chiusa per evitare sprechi. Sempre con un tempo prestavilito, parte anche una notifica che ricorda che la finistra è aperta. Si può impostare anche un altre timer ancora al termine del quale parte anche un avviso con Alexa.

### Impostazioni <img width="37" alt="Screenshot 2023-12-02 alle 10 04 16" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/855e3240-d447-48db-ab92-6ba09f1702f6">
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
<img width="415" alt="Screenshot 2023-11-21 alle 17 01 59" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/1d7f31d6-1bf4-4ff9-a69c-0f6463216a92">
Nel cerchio grade è indicata la temperatura attuale della stanza, il tasso di umidità relatica. Ne quadrato sopra invece è indicata la temperatura target.
E' possibile intervenire manualmente sulla temperatura con + e - oppure spegnere la valvola con il tastino in alto a sinistra.
Per quanto rigurada le statistiche credo che non ci siano bisogno di spiegazioni, mentre il grafico indica con il rosso la temperatura della stanza, con il nero la temperatura esterna e con le colonne l'apertura della valvola. 

Quando la valvola è chiusa si colora di nero, nel casso fosse disconnessa diventa grigia altrimenti bianca. Quando invece è in funzione, viene aggiunto uno sfondo inconfondibile più l'icona della fiamma.

## Pannllo generale 
<img width="384" alt="Screenshot 2023-12-02 alle 10 22 10" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/324937c0-4330-4880-a5cc-ae72a007a545">
In questo pannello abbiamo:


- il conteggio in tempo reale di quante valvole sono accese.
- Il totale delle ore giornaliere di funzionamente dell'impianto.
- Tasto per impostare la modalità fuori casa. <img width="45" alt="Screenshot 2023-12-02 alle 10 25 41" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/14651226-b770-439b-94b2-81b15ac69a4a">
- Tasto per impostare la modalità in casa. <img width="45" alt="Screenshot 2023-12-02 alle 10 25 46" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/c62020ee-7b29-4ee1-8ba1-6b46252e81a5">
- Tasto per spengere tutte le valvole. <img width="45" alt="Screenshot 2023-12-02 alle 10 25 50" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/ea121c48-9661-4290-8bd6-ea7d1c58390f">

Con questi 3 tasti invece abbiamo rispetticamente:
<img width="199" alt="Screenshot 2023-12-02 alle 10 33 14" src="https://github.com/Home-Assistant-Pro-Team/Benessere/assets/48358142/5a2b0c40-07e8-4dae-8d23-6380b7e4aae2">

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
