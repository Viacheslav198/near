# Генерация ключей и их резервное копирование

Первый шаг - перейти на https://wallet.betanet.near.org/, чтобы создать свою учетную запись. 

## 1. Генерация ключей
Для начала нам понадобится установить Node.js

```
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
source ~/.bashrc
nvm install v12.0.0
```

Далее устанавливаем Near Shell:

```
npm i -g near-shell
```
Затем настройте доступ к сети betanet:

```
export NODE_ENV=betanet
```

Затем войдите в свою учетную запись:

```
near login
```
Затем, если экран не открывается автоматически в вашем браузере, вы должны скопировать и вставить в браузер ссылку, которая позволит вам авторизировать ваш аккаунт

![pic](https://github.com/Viacheslav198/images/blob/master/22.png?raw=false)

Нажимаем «ALLOW» 

![pic](https://github.com/Viacheslav198/images/blob/master/23.png?raw=false)

Вводим имя вашего аккаунта и подтверждаем «CONFIRM»

![pic](https://github.com/Viacheslav198/images/blob/master/24.png?raw=false)

После успешного входа, ключи сгенирируются в формате json по пути:

```
~/.near-credentials/betanet
```

## 2.Резервное копирование ключей

Создаем папку Backup и копируем туда папку .nano-credentials

```
mkdir backup
sudo cp -r ~/.nano-credentials ~/backup
```
Папка скопирована:

![pic](https://github.com/Viacheslav198/images/blob/master/25.png?raw=false)

## Для переноса авторизации на новый сервер делаем следующее:

1. Устаналиваем Near-shell, Node.js на новый сервер
2. Переносим со старого сервера папку .nano-credentials в домашнюю директорию нового сервера

Чтобы убедиться, что все сделано правильно, проводим транзакцию

![pic](https://github.com/Viacheslav198/images/blob/master/26.png?raw=false)

Транзакция прошла успешно! 
