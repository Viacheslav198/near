# Iscriviti al tuo nodo

Crea un nuovo portafoglio qui: https://wallet.betanet.near.org/

Per ottenere 75.000 token nel tuo portafoglio, devi registrarti qui: https://nearprotocol1001.typeform.com/to/TvvOMf ed effettuare una richiesta per accedere al file `VALIDATORS.md`. Vai qui: https://github.com/nearprotocol/stakewars/blob/update-validators-list/VALIDATORS.md e modificalo.

## Rimozione delle chiavi di accesso dal contratto
Ora è necessario rimuovere tutte le chiavi pubbliche dal pool in modo da non avere accesso e bloccare il contratto.

Esaminiamo tutte le chiavi di accesso disponibili:
```
near keys stakingPool_ID | grep public_key
```
Eliminiamo la chiave pubblica, quindi blocciamo il contratto:
```
near delete-key --accessKey <PUBKEY> --accountId stakingPoolID
```
`<PUBKEY>` - la chiave che abbiamo appreso in precedenza
`stakingPoolID` - pool di picchettamento dell'account

## Delega di token al contratto
Per prima cosa devi inviare i token al nostro contratto di picchettamento della piscina.

`stakingPool_ID` - pool a cui inviamo token
`account_ID` - account da cui inviamo token

Invieremo 100 token:
```
near call stakingPool_ID deposit '{}' --accountId account_ID --amount 100
```
Dopo di che devono essere stake:
```
near call stakingPool_ID stake '{"amount": "100000000000000000000000000"}' --accountId account_ID
```
## Verifica delle impostazioni corrette del pool
Per cominciare, verificheremo che i nostri token sono realmente picchettati. Vai all'esploratore `https://explorer.betanet.near.org/` e controlla

Controlliamo che saremo inclusi nell'elenco dei validatori:
```
near validators next| grep "stakingPool_ID"
```
Noda prenderà il 1 ° posto nella prossima era nella lista dei validatori

Mostra tutte le domande presentate:
```
near proposals
```
Mostra tutti i validatori correnti:
```
near validators current
```
Controlla il prezzo corrente di un luogo per diventare un validatore:
```
near validators next | grep 'seat price'
```
Controlla il saldo bloccato sul nostro pool di picchettamento per assicurarti di avere davvero abbastanza token:
```
near state stakingPool_ID
```
Successivamente, è necessario verificare se abbiamo effettivamente eliminato le chiavi del nostro contratto di pool di gestione temporanea:
```
near keys stakingPool_ID | grep public_key
```
Apriamo i log e vediamo se siamo un validatore:
```
nearup logs -f
```
'V' - significa validatore

Puoi anche inviare una richiesta a JSON RPC per scoprire un sacco di informazioni sui validatori attuali e futuri:
```
curl -d '{"jsonrpc": "2.0", "method": "validators", "id": "dontcare", "params": [null]}' -H 'Content-Type: application/json' https://rpc.betanet.near.org -v | jq
near validators next | grep 'Kicked' 
```

Invece dell'ultimo elemento `jq` possiamo sostituire `jq .result.current_proposals` e vedere le transazioni di picchettamento in sospeso:

## IMPORTANTE!
Controlla che `stakingPool_ID` e il tuo nodo utilizzino lo stesso identificatore e `public_key` altrimenti il validatore non produrrà blocchi!

Utilizzando il comando, `near validators current|grep '0%'` è possibile visualizzare i validatori che hanno prodotto 0 blocchi. Ciò significa che saranno espulsi nella prossima era. E i loro delegati perderanno i premi.

Scopri chi verrà espulso nella prossima era `near validators next | grep ‘Kicked’`

Utilizzare il comando `cat ~/.near/betanet/validator_key.json | grep ‘public_key\|stakingPool_ID’` e assicurarsi che la chiave corrisponda esattamente al valore che abbiamo esaminato attraverso l'RPC JSON sopra
