# Installa vicino
Le istruzioni per l'installazione e l'esecuzione in nearup sono disponibili all'indirizzo https://github.com/near/nearup . Seguire i passaggi sul collegamento fornito per l'installazione nearup. Non avviare ancora il nearup . Eseguiremo quasi utilizzando il client nearcore compilato.

# Clone vicino a Beta Branch
Stakewars II utilizza il ramo beta nearcore per il nodo validatore. Si consiglia di clonare il codice in una cartella identificata dall'ultimo hash di commit (ad esempio se l'ultimo hash di build è 74bfab78, nel comando git utilizzare il nome della cartella come nearcore-74bfab78). È più facile tornare rapidamente tra le versioni di nearcore in caso di problemi con una versione particolare. Per mantenere separate le versioni nearcore (è disponibile una versione settimanale del codice aggiornato nel ramo beta) puoi seguire questi passaggi:

```
git clone -b beta https://github.com/nearprotocol/nearcore.git
```
Dopo aver clonato correttamente il codice, utilizzare il comando `cd` per passare alla directory più recente. Il nome della cartella fornito di seguito è solo per riferimento. Dovrai sostituirlo con il nome corretto con cui è stato creato.
```
cd ~/nearcore
```
Una volta nella cartella, verifica di essere effettivamente sul ramo beta
```
git branch
```
Se vedi qualcosa di lile `*beta` nell'output significa che hai estratto correttamente il ramo beta.

# Crea nearcore
Puoi creare nearcore in due modi diversi. Puoi scegliere di eseguire una build completa usando make releaseoppure puoi semplicemente eseguire il comando seguente che esegue una sola build cargo per nearcore/neard.
```
cargo build -p neard --release
```
Questo comando, dopo il completamento con esito positivo, crea le binarie client nearcore nella `nearcore/target/release` directory.

La build limitata di cui sopra potrebbe risparmiare un po 'di tempo in quanto evita di seguire le build aggiuntive se si utilizza `make release`(fare riferimento a Makefile in nearcore).
```
	cargo build -p keypair-generator --release
	cargo build -p genesis-csv-to-json --release
	cargo build -p near-vm-runner-standalone --release
	cargo build -p state-viewer --release
	cargo build -p store-validator --release
```
# Esegui nearup Utilizzando Client nearcore compilato
Ora puoi avviare il nodo validatore su betanet usando il seguente comando (cambia il nome della cartella nearcore in base alla versione che vuoi eseguire. Nearcore-613953cf è usato solo come riferimento nel comando seguente).
```
nearup betanet --nodocker --binary-path $HOME/nearcore-74bfab78/target/release
```
Quando si esegue il comando nearup per la prima volta, viene richiesto un ID account. Nella riga di comando verrà visualizzato un messaggio simile al seguente
```
Enter your account ID (leave empty if not going to be a validator):
```
Immettere l'ID che verrà utilizzato per creare e distribuire un contratto di pool di picchettamento nel passaggio successivo. Ad esempio, ho usato il nome del mio contratto di pool di picchettamento, ovvero `staking-pool.betanet` (da creare e distribuire nella sezione successiva).

Non dimenticare di aggiungere `staking-pool.betanet` all'ID. Ciò è necessario se si desidera utilizzare l'interfaccia utente della fabbrica del contratto di picchettamento per generare e distribuire il contratto del pool di picchettamento. Ho notato questo ID poiché lo useremo per generare e distribuire il contratto di pool di picchettamento nella sezione successiva.

Dopo aver fornito l'ID account e premuto Invio, il nodo verrà avviato e nella riga di comando verranno visualizzati messaggi simili a quelli seguenti
```
Generating node key...
PK: ed25519:...
Node key generated
Generating validator key...
PK: ed25519:...
Validator key generated
Stake for user 'xxxxxxx' with 'ed25519:...'
Starting NEAR client...
Node is running! 
To check logs call: `nearup logs` or `nearup logs --follow`
```
Puoi controllare i log per vedere se il nearup è stato avviato senza errori usando `nearup logs -f`

# Distribuire un pool di picchettamento utilizzando Factory di picchettamento

È possibile utilizzare l'interfaccia utente di Staking Pool Factory per distribuire un pool di picchettamento. Apri https://near-examples.github.io/staking-pool-factory/ e accedi utilizzando il tuo portafoglio betanet. Nell'interfaccia utente di Staking Pool Factory verranno visualizzati i seguenti campi:

Staking Pool ID

In questo campo inserisci lo stesso ID che hai usato durante l'avvio vicino al passaggio precedente

questo è molto importante che stai inserendo lo stesso ID che hai usato all'avvio del vicino

Owner ID

Immettere l'ID del portafoglio dell'account che riscuoterà la commissione per questo pool di picchettamento (è possibile utilizzare il portafoglio betanet creato per Stakewars II)

Initial Staking Public Key

Immettere la chiave pubblica generata da nearup al momento dell'avvio. È possibile ottenere la chiave da chiave pubblica campo dall'uscita del `more ~/.near/betanet/validator_key.json | grep 'public_key\|account_id'` sul server (output di esempio fornito di seguito - utilizzare solo il valore dopo ed25519: )
```
  "account_id": "staking-pool.betanet",
  "public_key": "ed25519:..."
```
Frazione della commissione di ricompensa iniziale

Questa è l'età% della commissione che accumulerai sui premi generati per la gestione di questo pool di puntate. Inserisci l'importo che desideri addebitare come commissione.

Dopo aver inserito e ricontrollato i valori, fare clic sul pulsante "Crea pool di picchettamento". Richiede almeno 30 Near nel tuo account per avviare questa transazione. Se il contratto viene creato correttamente, vedrai un messaggio simile a quello che segue nella parte superiore della pagina.

Creato con successo il tuo pool di picchettamento @staking-pool.betanet

Il pool di puntate appena creato è disponibile per depositi e puntate ora. Se la tua piscina di picchettamento ha abbastanza NEAR per ottenere un posto come validatore, apparirà come un valiadtor in circa 6 ore (2 epoche). Puoi avere un'idea del numero approssimativo di NEAR richiesto per ottenere un set come validatore eseguendo il comando seguente:
```
near validators next | grep "seat price"
```
Assicurati che la shell quasi sul tuo computer stia usando Betanet. Puoi fare in modo che near-shell usi betanet:
```
export NODE_ENV=betanet
```
# Deposito e picchettamento alla piscina di picchettamento
È possibile depositare e puntare al contratto appena distribuito utilizzando i seguenti comandi.
```
near call <name of the stake pool you created earlier> deposit '{}' --accountId <wallet from where you want to deposit> --amount <amount you want to deposit>
```
Di seguito è riportato un esempio di come ho depositato vicino al mio pool di picchettaggio dal mio portafoglio betanet Near.
```
near call staking-pool.betanet deposit '{}' --accountId USER.betanet --amount 3000
```
Se il deposito è andato a buon fine vedrai un messaggio simile al seguente.
```
Scheduling a call: staking-pool.betanet.deposit({}) with attached 3000 NEAR
[staking-pool.betanet]: @USER.betanet deposited 3000000000000000000000000000. New unstaked balance is 3000000000000000000000000000
''
```
Ora puoi puntare l'importo depositato usando il seguente comando
```
near call <name of the stake pool you deposited near token to> stake '{"amount": "<amount you want to stake>"}' --accountId <wallet id from where you made the deposit>
```
Ho usato il seguente comando per puntare al mio pool di picchettamento.
```
near call staking-pool.betanet stake '{"amount": "3000000000000000000000000000"}' --accountId USER.betanet
```
Se la chiamata di picchettamento ha esito positivo, verrà visualizzato un messaggio simile al seguente.
```
Scheduling a call: staking-pool.betanet.stake({"amount": "3000000000000000000000000000"})
[staking-pool.betanet]: @USER.betanet staking 3000000000000000000000000000. Received 2999999996874265166646757757 new staking shares. Total 0 unstaked balance and 2999999996874390698166169529 staking shares
[staking-pool.betanet]: Contract total staked balance is 3030000000001254315194165001. Total number of shares 3029999996874389698166169529
''
```
# I dettagli della mia piscina di picchettamento

Sto eseguendo @mutedtommy-staking-pool-2.stakehouse.betanet su betanet usando questi passaggi. Se si desidera delegare al mio pool di picchettamento, utilizzare i passaggi indicati nella sezione precedente (Depositing and Staking to the Staking Pool).

Qualsiasi suggerimento per migliorare il documento è il benvenuto.
