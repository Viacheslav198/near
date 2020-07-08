# Как управлять своим стейкинг пулом в сети Near

Итак, у вас уже есть свой пул ставок. Настало время понять как управлять им. Начнем!

```
stakingPool_ID — аккаунт стейкинг пула
account_ID —главный аккаунт
Public key — ключ на который хотите заменить
```

Изменить открытый ключ валидатора:
```
near call stakingPool_ID update_staking_key ‘{“stake_public_key”:”Public key”}’ — accountId account_ID
```

После внесенных изменений, можно проверить, какой ключ будет использован в следующей эпохе:
```
curl -d ‘{“jsonrpc”: “2.0”, “method”: “validators”, “id”: “dontcare”, “params”: [null]}’ -H ‘Content-Type: application/json’ https://rpc.betanet.near.org | jq -c ‘.result.current_proposals[] | select(.account_id | contains (“account_ID”))’
```
Не забудьте заменить account_ID!

Обновить текущую награду:
```
near call stakingPool_ID ping '{}' --accountId account_ID
```
```
ping — вызывает внутреннюю функцию для распределения вознаграждений, если сменилась эпоха. В этом случае контракт будет пересмотрен.
```

Если вы хотите снять токены со стейкинга пула, то необходимо сначала проверить, какое кол-во токенов доступно для съема:
```
near view stakingPool_ID get_account_total_balance '{"account_id": "account_ID"}'
```
После чего произвести анстейк токенов:
```
near call stakingPool_ID unstake '{"amount": "100000000000000000000000000"}' --accountId account_ID
```
Спустя 3 эпохи токены можно будет вывести из пула:
```
near call stakingPool_ID withdraw '{"amount": "100000000000000000000000000"}' --accountId account_ID
```
Остальные методы просмотра:

## 1.Просмотреть доступные токены для вывода:
```
near view stakingPool_ID is_account_unstaked_balance_available '{"account_id": "account_ID"}'
```
## 2.Узнать создателя стейкинг пула:
```
near view stakingPool_ID get_owner_id '{}'
```
## 3.Проверить баланс токенов, которые можно анстейкнуть:
```
near view stakingPool_ID get_account_unstaked_balance '{"account_id": "account_ID"}'
```
## 4.Проверить общий баланс токенов в пуле:
```
near view stakingPool_ID get_account_total_balance '{"account_id": "account_ID"}'
```
## 5.Проверить общий баланс стейкнутых токенов контракт пула:
```
near view stakingPool_ID get_total_staked_balance '{}'
```
## 6.Узнать баланс застейканых токенов:
```
near view stakingPool_ID get_account_staked_balance '{"account_id": "account_ID"}'
```
## 7.Посмотреть текущую комиссию пула:
```
near view stakingPool_ID get_reward_fee_fraction '{}'
```
## 8.Просмотреть ключ стейкинг пула:
```
near view stakingPool_ID get_staking_key '{}'
```
