# Creazione di un portafoglio NEAR
Per creare un portafoglio NEAR, basta visitare il sito https://wallet.betanet.near.org/ e inserire il login che si desidera utilizzare

Selezioniamo il metodo di recupero dell'account, in questo esempio utilizziamo il metodo di recupero usando le frasi seed, registriamo la frase e premiamo "CONTINUE"

Quindi entriamo nel nostro portafoglio, dove
saranno disponibili informazioni come: Informazioni generali sul portafoglio, trasferimento e ricezione di token, nonché i nostri dispositivi autorizzati e le chiavi pubbliche.

## Installazione di Near-Shell
Per una maggiore sicurezza, si consiglia l'installazione di Near-Shell su un server separato.
Aggiorniamo e installiamo tutti i pacchetti necessari:
```
sudo apt-get update
sudo apt install -y git build-essential
```
Per una corretta installazione di Near-Shell, è necessario installare Node.js almeno alla versione 10.
Installa Node.js e npm:
```
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
source ~/.bashrc
nvm install v12.0.0
```
Verifica delle versioni installate di Node.js e npm:
```
node -v
npm -v
```

Installa Near Shell:
```
npm i -g near-shell --unsafe-perm
```
Vai alla rete betanet:
```
export NODE_ENV=betanet
```
Ogni volta che ti connetti al server, devi cambiare la rete, quindi puoi farlo senza scrivere il comando:
```
echo -n "export NODE_ENV=betanet" >> ~/.profile
source ~/.profile
```
Ora, per impostazione predefinita, la connessione andrà alla rete betanet
Se devi passare a testnet, mainnet, vai su .profile e cambia betanet sulla rete desiderata:
```
cd && nano .profile 
source ~ / .profile
```
Accedi al tuo account:
```
near login
```
Near ti chiederà di autorizzarlo nel nostro account, copiare il link e fare clic su di esso. Devi seguire il link dallo stesso browser in cui abbiamo inserito il tuo portafoglio Near!

Seleziona il portafoglio da cui vogliamo accedere e fai clic su ALLOW:

Successivamente, inserisci lo stesso portafoglio manualmente e fai clic su "CONFIRM":

Se si verifica un errore, allora va bene, abbiamo dato correttamente l'accesso per accedere e possiamo accedere al nostro account.

Inserisci il login nella console senza @ e premi Enter:

Crea un account pool con un saldo iniziale di 40 N per il picchettamento:
```
near create_account stakingPool_ID --masterAccount=account_ID --initialBalance 40
```
`stakingPool_ID` - nome del pool per il picchettamento
`account_ID` - proprietario dell'account picchettamento piscina, il tuo account principale
`initialBalance` - saldo iniziale, se non specificato, otteniamo un errore:
```
Prova di creare un pool senza equilibrio
```
Visualizza le chiavi create:
```
ls ~/.near-credentials/betanet
```
## Installazione e configurazione del nodo NEAR
Se si utilizza un firewall, è necessario aprire la porta 24567 per tutti gli indirizzi IP (0.0.0.0/0)

Aggiorna il nostro sistema:
```
sudo apt-get update
sudo apt-get dist-upgrade
```
Installa tutti i pacchetti necessari:
```
sudo apt install -y python3 git curl make llvm clang jq 
```
Quindi, installa Rust:
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs/ | sh
source $HOME/.cargo/env
```
Crea un file di scambio da 4 GB:
```
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
Clona nearcore dal ramo beta sul nostro server:
```
git clone --branch beta https://github.com/nearprotocol/nearcore.git
```
Vai alla cartella nearcore e verifica che abbiamo un ramo beta:
```
cd nearcore/ && git branch
```
Compiliamo:
```
make release
```
Installa Nearup:
```
cd
curl --proto '=https' --tlsv1.2 -sSfL https://up.near.dev | python3
source $HOME/.nearup/env
```
Esegui il nodo:
```
nearup betanet --nodocker --binary-path ~/nearcore/target/release/
```
Inserisci il nostro account pool in questo campo:
```
Enter your account ID (leave empty if not going to be a validator):
```
Il nodo è in esecuzione, le chiavi vengono generate
Successivamente, devi esportare la chiave pubblica, ne avremo bisogno per configurare il nostro pool:
```
cat ~/.near/betanet/validator_key.json | grep 'public_key\|account_id'
```
Dopo un avvio riuscito, devi attendere che il nostro nodo sia completamente sincronizzato.
È possibile visualizzare i registri con il seguente comando:
```
nearup logs --follow
```
Sincronizzazione nodo

Puoi controllare la versione corrente del server e il nostro nodo con il seguente comando:
```
echo rpc.betanet.near.org; curl -s https://rpc.betanet.near.org/status | jq .version; echo;echo 127.0.0.1:3030; curl -s http://127.0.0.1:3030/status | jq .version
```
Il nodo è configurato correttamente.
Aggiornamento nodo:
```
cd nearcore && git pull
make release
```
## Impostazione di un pool di picchettamento
Ora puoi iniziare il processo di implementazione di un contratto per un pool di picchettamento.

Imposta Rustup:
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs/ | sh
export PATH="$HOME/.cargo/bin:$PATH"
```
Aggiornare:
```
rustup update stable
```
Imposta il target wasm32 per Rustup:
```
rustup +stable target add wasm32-unknown-unknown
```
Cloniamo ed espandiamo il contratto di pool:
```
git clone https://github.com/near/initial-contracts && cd initial-contracts/staking-pool
```
Creiamo il nostro contratto di pool di picchettamento:
```
./build.sh
```
Espandi il contratto:
```
near deploy --accountId=stakingPool_ID --wasmFile=initial-contracts/staking-pool/res/staking_pool.wasm
```
`stakingPool_ID` - account pool di picchettamento I
percorsi di `staking_pool.wasm` possono variare!

Inizializziamo l'account del pool di picchettamento:
```
near call stakingPool_ID new '{"owner_id": "account_ID", "stake_public_key": "My_stake_public_key", "reward_fee_fraction": {"numerator": 10, "denominator": 100}}' --account_id stakingPool_ID
```
`stakingPool_ID` - account di pool di account
`account_id_ID` - sostituiamo l' account
`"My_stake_public_key"` - principale con quello che abbiamo scritto in precedenza, è possibile registrare senza prefisso `ed25519:`, è possibile utilizzarlo

Siamo stati respinti perché non abbiamo abbastanza NEAR

Per scoprire qual è il numero minimo di token richiesti per essere accettati, puoi scrivere il comando:
```
near proposals | grep "seat price"
```
Dopo che ci accettano, devi aspettare 2 epoche (~ 6 ore) e diventeremo validatori

Ora è necessario rimuovere tutte le chiavi pubbliche dal pool in modo da non avere accesso e bloccare il contratto.

Esaminiamo tutte le chiavi di accesso disponibili:
```
near keys stakingPool_ID | grep public_key
```
Il nostro contratto ora ha lo stato non bloccato
Eliminiamo questa chiave pubblica, quindi blocciamo il contratto:
```
near delete-key --accessKey <PUBKEY> --accountId stakingPoolID
```
La chiave è stata eliminata correttamente, ora il nostro contratto ha lo stato LOCKED

Proviamo a inviare alcuni token al pool di picchettamento:
```
near call stakingPool_ID deposit '{}' --accountId account_ID --amount 100
```
Quindi trasformali in una bistecca:
```
near call stakingPool_ID stake '{"amount": "100000000000000000000000000"}' --accountId account_ID
```
Aggiorna picchettamento corrente:
```
near call stakingPool_ID ping '{}' --accountId account_ID
```
`stakingPool_ID` - pool di picchettamento
`account_ID` - il tuo account betanet principale
