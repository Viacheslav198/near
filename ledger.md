# Использование приложения NEAR в аппаратном кошельке Ledger Nano S

Приложение Ledger Live имеет совместимость с Windows 8+, macOS 10.10+, Linux. 64-bits desktop computers excluding ARM processors.

## Настройка нового аппаратного кошелька Ledger Nano S
1. Заходим на страницу https://www.ledger.com/ledger-live/download/

2. Выбираем свою ОС и скачиваем приложение
![pic](https://github.com/Viacheslav198/images/blob/master/1.png?raw=false)

3. Как только установочный файл скачается, его нужно установить на Ваш компьютер.

4. Далее, находим установленное приложение на компьютере и заходим в него.

5. Откроется интерфейс Ledger Live

6. Нажимаем “Get started”.
![pic](https://github.com/Viacheslav198/images/blob/master/3.png?raw=false)

7. В появившемся списке выбираем:
Initialize a new Ledger device (настройка нового кошелька Ledger)
![pic](https://github.com/Viacheslav198/images/blob/master/4.png?raw=false)

8. Выбираем ваш кошелек Ledger Nano S и нажать “Continue”
![pic](https://github.com/Viacheslav198/images/blob/master/5.png?raw=false)

9. Далее нужно подключить ваш кошелек Ledger nano s к компьютеру с помощью кабеля который входит в комплект. В сообщении на экране написано, что нужно после подключения кошелька к компьютеру выбрать “Configure a new device”, и придумать ПИН код от 4 до 8 цифр, чтобы подтвердить ПИН код нужно нажать на “галочку”. Также на этом экране написано, что с помощью ПИН кода вы будете заходить в сам кошелек, что 8 цифр для ПИН кода поможет максимально обезопасить кошелек и самое важное “НИКОГДА НЕ ИСПОЛЬЗУЙТЕ КОШЕЛЕК ЕСЛИ В КОРОБКЕ УЖЕ НАПИСАН ПИН КОД ИЛИ ПРИВАТНАЯ ФРАЗА 24 СЛОВА”, ПИН код придумываете только вы, а приватный ключ из 24 слов, кошелек генерирует автоматически при первой настройке кошелька.
![pic](https://github.com/Viacheslav198/images/blob/master/6.png?raw=false)

10. На следующем экране написано, как нужно записывать вашу приватную фразу из 24 слов, нужно записывать их с кошелька, строго в порядке их появления, 1 WORD, 2 WORD и так далее. Храните ваш приватный ключ в надежном месте, это самое ценное, что выдает аппаратный кошелек.
![pic](https://github.com/Viacheslav198/images/blob/master/7.png?raw=false)

11. Далее, приложение будет спрашивать вас, все ли вы сделали для настройки кошелька: “Вы придумали Пин код?”, “Вы записали вашу приватную фразу?”, “Ваш кошелек оригинальный?” - после этого нужно нажать на кнопку “Check now” и программа начнет проверять ваш кошелек на оригинальность.
![pic](https://github.com/Viacheslav198/images/blob/master/8.png?raw=false)
![pic](https://github.com/Viacheslav198/images/blob/master/9.png?raw=false)
После проверки в приложение будет написано “Your device is genuine” - это значит, что ваш кошелек прошел проверку и он настоящий. далее нажимаем “Continue”

12. Приложение предложит вам придумать пароль для входа в приложение в оффлайн режиме, это делается опционально, чтобы никто не смог посмотреть ваш баланс, историю транзакций.
![pic](https://github.com/Viacheslav198/images/blob/master/11.png?raw=false)

13. В следующем окне будет написано, хотите ли вы чтобы технический отдел Ledger мог анализировать и получать проблемы с которыми сталкиваются пользователи, после нажимаем “Continue”
![pic](https://github.com/Viacheslav198/images/blob/master/12.png?raw=false)

14. Вот и вы и настроили приложение Ledger Live, нажмите кнопку “Open Ledger Live”.
![pic](https://github.com/Viacheslav198/images/blob/master/13.png?raw=false)

## Добавление приложения NEAR в Ledger Nano S

1. Нажимаем шестеренку в правом верхнем углу 
![pic](https://github.com/Viacheslav198/images/blob/master/14.png?raw=false)

2. Переходим во вкладку Experimental Features и включаем режим Developer Mode
![pic](https://github.com/Viacheslav198/images/blob/master/15.png?raw=false)

3. Далее нажимаем "Manager" в левой части экрана

4. В каталоге приложений находим NEAR и устанавливаем его
![pic](https://github.com/Viacheslav198/images/blob/master/16.png?raw=false)

## Подключение к аккаунту NEAR с помощью Ledger Nano S 

1. Заходим в кошелек NEAR
2. В правом верхнем углу выбераем "Profile" 
3. В открывшейся странице в нижнем левом углу, рядом с Ledger Hardware Wallet нажимаем "ENABLE"
![pic](https://github.com/Viacheslav198/images/blob/master/17.png?raw=false)

4. Входим в приложение NEAR на нашем Ledger Nano S 
![pic](https://github.com/Viacheslav198/images/blob/master/19.jpg?raw=false)

5. Нажимаем Connect to Ledger на  веб странице
![pic](https://github.com/Viacheslav198/images/blob/master/18.png?raw=false)

6. Далее потверждаем Public key нажав на правую кнопку устройства
![pic](https://github.com/Viacheslav198/images/blob/master/20.jpg?raw=false)

7. Ledger Nano S  успешно подключен к нашему аккаунту NEAR 
![pic](https://github.com/Viacheslav198/images/blob/master/21.png?raw=false)
Рекомендуется удалить все существующие ключи, так как поддержка нескольких ключей повышает уязвимость вашего аккаунта.
