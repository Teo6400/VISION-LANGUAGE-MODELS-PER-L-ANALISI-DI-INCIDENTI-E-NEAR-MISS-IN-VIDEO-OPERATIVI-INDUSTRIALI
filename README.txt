================================================================================
README — Cartella di lavoro
Tesi: analisi di incidenti e near miss industriali tramite Vision-Language Models
Ultimo aggiornamento: <GG/MM/AAAA>
================================================================================

1. SINTESI
----------
Questa cartella contiene tutto il materiale operativo della tesi: il codice per
l'analisi dei video con i Vision-Language Models, i video di incidenti e near
miss, gli output sperimentali e le annotazioni di ground truth. La letteratura e
i documenti di tesi sono in una cartella separata e non servono per eseguire il
codice.

Scopo del presente file: spiegare a una persona o a un assistente AI che lavora
dentro questa cartella cosa contiene, come è organizzata e quale compito deve
svolgere.

2. CONTESTO E OBIETTIVO DEL PROGETTO
------------------------------------
La tesi valuta l'uso dei Vision-Language Models nell'analisi di video di
incidenti e near miss in ambito industriale. I tre obiettivi operativi sono:

  a) identificare automaticamente azioni e comportamenti potenzialmente
     pericolosi nei video;
  b) ricostruire la sequenza degli eventi che conduce all'incidente o al near
     miss;
  c) estrarre informazioni utili per l'analisi delle cause e la prevenzione.

Il lavoro consiste nel progettare e testare diverse strategie di prompting,
applicarle a un dataset di video e valutare i risultati in termini di
accuratezza, coerenza e utilità per l'analisi della sicurezza.

3. STRUTTURA DELLE CARTELLE
---------------------------
<Radice>
|-- codici/          script e notebook per l'analisi con i VLM
|-- video/           video di incidenti e near miss (input)
|-- risultati/       output delle esecuzioni: json, csv, metriche, figure
|-- ground_truth/    annotazioni manuali di riferimento
|-- docs/            note, appunti, materiale di supporto
`-- README.txt       questo file

Per rigenerare l'albero completo:

  tree -L 3 -I '.git|__pycache__|.ipynb_checkpoints|venv|node_modules'

L'opzione -I esclude dal listato i pattern indicati
[29].

4. MAPPA DELLE CARTELLE
-----------------------
codici/
  Contiene gli script Python e i notebook. Qui vive la logica sperimentale:
  costruzione dei prompt, chiamata ai modelli, parsing delle risposte,
  calcolo delle metriche. Nessun file di questa cartella va modificato senza
  motivo esplicito.

video/
  Video sorgente in <formato: mp4/avi>. Sono gli input dell'analisi e non
  vanno rinominati, perché i nomi sono la chiave usata dal codice e dalla
  ground truth.

risultati/
  Output generati dalle esecuzioni. Struttura tipica: una sottocartella per
  ogni configurazione di prompt o di modello, con dentro risposte grezze,
  output parsati e metriche aggregate. È materiale rigenerabile: va trattato
  come derivato, non come sorgente.

ground_truth/
  Annotazioni manuali usate come riferimento per la valutazione. Formato
  tabellare, una riga per evento o per clip.

docs/
  Note di lavoro, appunti, materiale di supporto non eseguibile.

5. FLUSSO DI LAVORO
-------------------
  video/  ->  codici/ (estrazione frame, prompting, parsing)  ->  risultati/

Passi tipici:

  1. si sceglie un video o una clip da video/;
  2. lo script in codici/ costruisce il prompt e interroga il modello;
  3. la risposta viene salvata in risultati/ in forma grezza;
  4. un secondo passaggio fa il parsing e calcola le metriche rispetto a
     ground_truth/;
  5. le metriche aggregate finiscono in un file di riepilogo per
     configurazione.

Ogni esperimento deve essere identificabile: nome del modello, versione del
prompt, set di video usato, data dell'esecuzione.

6. INVENTARIO DEI FILE
----------------------
Per generare l'elenco aggiornato con percorsi e dimensioni:

  find . -type f -not -path './.git/*' -printf '%s\t%p\n' | sort -k2

Tabella da compilare per i file rilevanti:

  PERCORSO                      TIPO     RUOLO                          NOTE
  codici/<script>.py            script   <es. chiamata al modello>      <note>
  codici/<notebook>.ipynb       notebook <es. analisi metriche>        <note>
  video/<clip>.mp4              video    input dell'analisi            <note>
  risultati/<file>.json         output   risposte grezze del modello    <note>
  ground_truth/<file>.xlsx      dati     annotazioni di riferimento     <note>

7. VIDEO E DATASET
------------------
Per ogni video: nome, formato, durata, risoluzione, evento contenuto,
eventuale clip di interesse (inizio/fine) e ground truth associata.

Durata e risoluzione si leggono con ffprobe, senza aprire il file:

  # risoluzione (es. 1280x720)
  ffprobe -v error -select_streams v:0 -show_entries stream=width,height \
          -of csv=s=x:p=0 video.mp4

  # durata in secondi
  ffprobe -v error -show_entries format=duration \
          -of default=noprint_wrappers=1:nokey=1 video.mp4

Entrambi i comandi restituiscono un solo valore numerico, utile per compilare
automaticamente una tabella di riepilogo [28][30].

8. RISULTATI E OUTPUT
---------------------
  - Risposte grezze del modello: <percorso e formato>
  - Output parsati: <percorso e formato>
  - Metriche per esecuzione: <percorso e quali metriche>
  - Figure e tabelle per la tesi: <percorso>

Nota sulla riproducibilità: un risultato senza modello, prompt e set di input
dichiarati non è riproducibile e va considerato provvisorio.

9. COME SI USA QUESTA CARTELLA
------------------------------
  1. Attivare l'ambiente Python del progetto (indicare qui il comando reale:
     <es. conda activate tesi> oppure <python -m venv .venv && source ...>).
  2. Verificare che le chiavi API siano in variabili d'ambiente, mai nel
     codice.
  3. Eseguire gli script nell'ordine indicato nella sezione 5.
  4. Controllare che gli output siano finiti in risultati/ nella sottocartella
     corretta prima di lanciare l'esecuzione successiva.

10. CONVENZIONI
---------------
  - Nomi dei file: <es. <modello>_<prompt>_<data>.json>
  - Encoding dei file di testo: UTF-8.
  - Separatore dei CSV: <virgola o punto e virgola>.
  - Identificativi degli eventi: <schema usato tra video e ground truth>.

11. COMPITO ASSEGNATO ALL'ASSISTENTE AI
---------------------------------------
L'assistente che lavora in questa cartella deve:

  a) leggere la struttura e questo README prima di qualsiasi azione;
  b) descrivere, confermare o correggere le sezioni 3-8 sulla base di ciò che
     trova realmente nel filesystem;
  c) non modificare, rinominare, spostare o cancellare file senza richiesta
     esplicita; può creare solo i file necessari al compito assegnato;
  d) non eseguire script, non lanciare modelli e non effettuare chiamate API
     senza autorizzazione, per evitare costi e risultati non tracciati;
  e) dichiarare esplicitamente ciò che non ha potuto verificare, invece di
     dedurlo;
  f) scrivere in italiano, in modo tecnico e asciutto.

12. PUNTI APERTI
----------------
  - <file o cartella da chiarire>
  - <output di cui non è nota la configurazione di generazione>
  - <duplicati o versioni multiple dello stesso file>

================================================================================
File generato e mantenuto manualmente. Se un'informazione non è verificabile,
va segnalata nella sezione 12 e non inventata.
================================================================================