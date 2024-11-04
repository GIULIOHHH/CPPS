# Italiano

A sinistra (in verde) ci sono i problemi.


Ogni problema ha due link:
1. Descrizione
2. Sottoposizione
## Descrizione
In alto c'è il tasto (in verde) per scaricare il testo del problema.
In basso ci sono dei template per i linguaggi di programmazione (in rosso) e dei file di input-output (di solito non molto utili)
![](Trash/Pasted%20image%2020241104221930.png)
Nella pagina delle sottomissioni:
Il pulsante per scegliere il file e il pulsante per sottomettere.
Il linguaggio di programmazione viene scelto in automatico.
![](Trash/Pasted%20image%2020241104222028.png)
Una volta sottoposto il problema cliccando su dettagli si possono vedere i risultati dei testcase

![](Trash/Pasted%20image%2020241104222805.png)

## Messaggi di errore

| Messaggio                                                                                                                                                                                                                 | Spiegazione                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| _Compilazione eseguita con successo_                                                                                                                                                                                      |                                                                              |
| _Compilazione fallita_ *////////////* _Esecuzione fallita a causa di codice di ritorno diverso da zero_                                                                                                                   | **Errore di codice**                                                         |
| _Compilazione interrotta perché fuori tempo massimo_                                                                                                                                                                      | Supera il tempo limite. Di solito **Errore di logica** o loop non terminato. |
| _Compilazione terminata dal segnale %s (può essere dovuto ad una violazione dei limiti di memoria)_                                */////////////////* _Execution killed (could be triggered by violating memory limits)_ | **Errore di logica**, il programma utilizza troppa memoria.                  |
|                                                                                                                                                                                                                           |                                                                              |
|                                                                                                                                                                                                                           |                                                                              |

# Spiegazione dei template
In ogni template ci sarà una sezione simile dove và inserito il codice:
![](Trash/Pasted%20image%2020241104222934.png)
## Inserimento manuale dei dati
Runnando il codice senza altre modifiche:
1. I dati di input verranno inseriti manualmente nel terminale
2. Il programma ritornerà nel terminale i dati di output.
## Testcase predefiniti
Togliendo i commenti a queste 2 linee di codice:
![](Trash/Pasted%20image%2020241104223051.png)
1. La prima farà si che i dati di input vengano presi da un file (nella stessa cartella del sorgente) con il nome *input.txt* (o il nome cambiato dall'utente.)
2. La seconda farà si che i dati di output vengano inseriti in un file chiamato *output.txt*, creato nella stessa cartella del sorgente.
I file di input si possono scaricare dalla pagina di [[#Descrizione]] del problema.