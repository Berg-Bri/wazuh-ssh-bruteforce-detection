# Teoria: SIEM, MITRE ATT&CK e strategie di logging

## Cos'è un SIEM / XDR

**SIEM (Security Information and Event Management):** soluzione centralizzata che raccoglie, analizza e correla i log generati da server, dispositivi di rete, firewall e applicazioni. Permette di identificare anomalie ed eventi di sicurezza in tempo reale, invece di dover controllare manualmente i log di ogni singolo sistema.

**XDR (Extended Detection and Response):** evoluzione del SIEM tradizionale. Oltre alla raccolta passiva dei log, integra funzionalità attive di risposta agli incidenti (_Active Response_), come l'isolamento di un host o il blocco automatico di un IP malevolo a livello di firewall, senza intervento manuale.

## Architettura di Wazuh

Wazuh si articola in tre componenti:

1. **Wazuh Indexer** — motore di ricerca e analisi dati (basato su OpenSearch) che memorizza e indicizza tutti gli eventi di sicurezza raccolti.
2. **Wazuh Server (Manager)** — il "cervello" della piattaforma: riceve i log, applica i decoder per estrarne informazioni strutturate, e confronta gli eventi con un motore di regole per generare alert.
3. **Wazuh Dashboard** — interfaccia web per visualizzazione grafica, threat hunting, gestione alert e report.

## Il framework MITRE ATT&CK

Matrice globale di conoscenza tattica usata per categorizzare le azioni degli attaccanti in modo standardizzato — permette a chi analizza un incidente di descriverlo con un linguaggio comune invece di descrizioni ad hoc.

In questo laboratorio, l'attività eseguita rientra in:

- **Tattica:** `Credential Access` (TA0006) — tecniche usate per rubare credenziali valide.
- **Tecnica:** `Brute Force` (T1110) — tentativi sistematici e ripetuti di indovinare credenziali valide tramite liste o combinazioni automatizzate.

## Logging strategy: agent-based vs syslog (agentless)

**Agent-based:** prevede l'installazione di un software leggero sull'host monitorato. Vantaggi: traffico cifrato TLS verso il manager, monitoraggio dell'integrità dei file (FIM), supporto a risposte attive (Active Response). Limite: richiede un sistema operativo compatibile con l'agent.

**Syslog (agentless, tipicamente UDP 514):** usato per dispositivi di rete (switch, router, firewall) o sistemi che non possono eseguire un agent — inclusi sistemi legacy non aggiornabili, come Metasploitable 2 in questo lab. È un protocollo privo di cifratura nativa (i log viaggiano in chiaro), ma è l'unica opzione quando l'installazione di codice nativo non è possibile.

Questo lab usa entrambe le strategie fianco a fianco: agent-based su Kali, syslog forwarding su Metasploitable — una situazione realistica in molte reti aziendali, dove coesistono sistemi moderni e infrastruttura legacy che non può essere aggiornata o sostituita a breve termine.