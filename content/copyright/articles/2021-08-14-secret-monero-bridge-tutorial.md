---
title: "Учебное пособие по секретному мосту Monero"
date: "2021-08-14"
categories:
  - "Статьи"
tags:
  - ""
thumbnail: "img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/01.png"
lead: "Секретный мост Monero (Secret Monero Bridge) работает в основной сети Secret Network и обеспечивает возможность связывать токены XMR с их приватным эквивалентом (sXMR), поддерживающим экосистему DeFi, в сети Secret Network.​"
pager: true
toc: true
sidebar: "right"
---

## _Вводная информация_

Это, в свою очередь, открывает возможность для торговли XMR на децентрализованной бирже (DEX) SecretSwap, возможность зарабатывать и получать награды за LP в качестве провайдера ликвидности, сохраняя при этом свою приверженность Monero.

Познакомиться с Secret Monero Bridge и попробовать поработать с ним можно по следующей ссылке: https://bridge.scrt.network (просто выберите Monero в раскрывающемся списке).

Узнать больше о Secret Network можно здесь: https://scrt.network.

## _Цель_

Цель настоящего руководства состоит в том, чтобы научить пользователей переносу Monero (XMR) в Secret Network и переводу sXMR обратно в Monero посредством Secret Monero Bridge, обеспечивающего доступ к экосистеме DeFi сети Secret Network.

В рамках данного руководства мы используем GUI-кошелёк Monero.

Предварительные условия:
- Наличие кошелька Monero, обеспечивающего функцию проверки ключа транзакций check_tx_key (доказательства платежа Monero), GUI-кошелька Monero или CLI-кошелька Monero (см. https://www.getmonero.org/downloads/). Переводы при использовании аппаратных кошельков требуют ручного подтверждения транзакций и нескольких дней на их совершение. Перед использованием моста настоятельно рекомендуется переместить токены в неаппаратный кошелёк.
- Установите кошелёк Keplr, кошелёк с браузерным расширением с открытым исходным кодом, поддерживающий экосистему Cosmos для совершения операций между блокчейнами. Приложение можно скачать по этой ссылке: https://wallet.keplr.app/. Храните токены XMR в своём кошельке Monero. SCRT храните в кошельке Keplr, чтобы использовать его при выплате комиссий за газ. Как вариант, вы можете воспользоваться сервисом для проведения свопов, поддерживаемым мостом (соответствующие инструкции можно найти в этом руководстве).

## _Инструкция_

### _Подключение к приложению Secret Monero Bridge_

1. Перейдите на страницу Secret Bridge по адресу https://bridge.scrt.network и выберите в раскрывающемся меню закладку Monero Bridge или же сразу перейдите непосредственно Secret Monero Bridge по адресу
https://ipfs.io/ipns/k51qzi5uqu5dhovcugri8aul3itkct8lvnodtnv2y3o1saotkjsa7q

\*	Обратите внимание, что в настоящее время Chrome является единственным поддерживаемым браузером.

![04](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/04.png)

2. Войдите в свое браузерное расширение Keplr, если вы ещё этого не сделали, и подтвердите соединение между вашим кошельком и мостом.

![05](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/05.png)

![06](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/06.png)

### _Создание ключа просмотра для sXMR_

Если у вас уже имеется ключ просмотра sXMR, пропустите этот шаг и перейдите к разделу «Внесение средств (создание sXMR в секретной сети)».

1. Перейдите к вкладке внесения средств в верхней части страницы.

![07](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/07.png)

2. Приложение Bridge просканирует ваш кошелёк Keplr и определит, есть ли у вас ключ просмотра sXMR. Если у вас его нет, мост предложит создать ключ просмотра.

![08](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/08.png)

3. Для этого вам потребуется наличие небольшого количества SCRT в вашем кошельке Keplr. SCRT можно приобрести на различных биржах и отправить их на свой кошелек, или, если вы не хотите пользоваться услугами централизованной биржи (CEX), следуйте инструкциям, которые приводятся в разделе часто задаваемых вопросов по Bridge в подразделе «SCRT: зачем они вам нужны и где их взять?»

![09](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/09.png)

![10](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/10.png)

4. Подтвердите создание ключа просмотра в Keplr и подтвердите транзакцию.

![11](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/11.png)

![12](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/12.png)

### _Внесение средств (создание sXMR в секретной сети)_

1. Перейдите к вкладке внесения средств в верхней части страницы моста.

![13](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/13.png)

2. Скопируйте адрес Monero Bridge в буфер обмена.

![14](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/14.png)

3. Отправьте XMR из вашего кошелька Monero на адрес моста. Обратите внимание, что мост берёт 0,0041 XMR в качестве комиссии за использование сервиса.

![15](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/15.png)

4. После того как транзакция будет успешно отправлена в блокчейн, скопируйте идентификатор транзакции (TxID) и вставьте его в поле Bridge.

![16](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/16.png)

![17](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/17.png)

5. TxKey можно получить, выбрав View Progress (Просмотр выполнения) в GUI-кошельке Monero, а затем, кликнув TxKey, можно открыть его. Скопируйте TxKey и вставьте его в поле Bridge.

![18](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/18.png)

![19](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/19.png)

![20](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/20.png)

6. Откройте расширение Keplr и убедитесь, что для него установлено значение Secret Network. По умолчанию оно открывается в экосистеме Cosmos. Кликом скопируйте свой секретный адрес. Вставьте его в поле Bridge и отправьте.

![21](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/21.png)

![22](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/22.png)

![23](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/23.png)

7. В качестве последнего шага отправьте электронное письмо с предоставленной информацией в Bridge. Чтобы автоматически открыть свой почтовый клиент, можно воспользоваться опцией Send Email (Отправить письмо). Как вариант, можно скопировать и вставить информацию в электронное письмо, которое вы создадите сами. В рамках данного руководства мы используем специально созданный для него аккаунт почтового сервиса Proton.

![24](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/24.png)

![25](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/25.png)

8. Подождите, пока средства поступят на ваш кошелёк Keplr. Для проверки требуется не менее 6 подтверждений в блокчейне Monero, после чего мост в течение 2 часов завершит транзакцию.

![26](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/26.png)

### _Вывод средств (сожжение sXMR в Secret Network и перевод XMR в сеть Monero)_

9. В верхней части страницы Secret Monero Bridge выберите опцию вывода.

![27](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/27.png)

10. Скопируйте свой адрес кошелька Monero и вставьте его в поле моста.

![28](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/28.png)

![29](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/29.png)

11. Введите сумму sXMR, которую желаете перевести. Обратите внимание, что при этом за использование сервиса будет удержана комиссия в размере 0,0041 XMR. Нажмите Submit (Отправить).

![30](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/30.png)

12. Подтвердите транзакцию в вашем кошельке Keplr.

![31](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/31.png)

13. В качестве последнего шага отправьте электронное письмо с предоставленной информацией в Bridge. Чтобы автоматически открыть свой почтовый клиент, можно воспользоваться опцией Send Email. Как вариант, можно скопировать и вставить информацию в электронное письмо, которое вы создадите сами. В рамках данного руководства мы используем специально созданный для него аккаунт почтового сервиса Proton.

![32](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/32.png)

![33](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/33.png)

14. Подождите, пока средства поступят на ваш кошелёк Monero. Для проверки требуется не менее 6 подтверждений в блокчейне Monero, после чего мост в течение 2 часов завершит транзакцию.

![34](/img/copyright/articles/2021-08-14-secret-monero-bridge-tutorial/34.png)

## _Что дальше?_

Надеюсь, мы помогли вам лучше понять, как пользоваться Secret Monero Bridge. В настоящее время уже абсолютно любой пользователь может создавать SecretXMR, обменивать и добывать эти токены или сжигать их для перевода обратно в Monero.

Для тех пользователей, кто уже знаком с DeFi и майнингом ликвидности, это новый привлекательный способ получить доход и воспользоваться возможностями сохранения приватности Monero, а также получить доступ к революционному миру Secret DeFi — вселенной, в которой приложения защищены от фронтраннинга и уже только по своей технической природе обеспечивают приватность!
Узнайте больше об экосистеме Secret Network можно здесь: https://scrt.network.

---

_Источник: [Secret Monero Bridge Tutorial](https://medium.com/@secretnetwork/secret-monero-bridge-tutorial-22c5eab8bcac)_

_Перевод: [Mr. Pickles](https://t.me/v1docq47)_  
_Коррекция: [Kukima](https://t.me/Kukima)_
