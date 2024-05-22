---
title: "Haveno DEX - Прямой обмен фиатной валюты на Monero"
categories:
  - "Руководства"
comments: false
authorbox: false
pager: true
toc: true
mathjax: true
sidebar: "right"
---

![01](/img/manuals/haveno-client-f2f/01.png)

В этом руководстве мы расскажем о том, как совершить обмен фиатной валюты на Monero с помощью новой (и долгожданной!) **децентрализованной биржи Haveno**, используя Face-to-Face (F2F) метод оплаты.

![02](/img/manuals/haveno-client-f2f/02.png)

_Примечание: я не рекомендую использовать Face-to-Face (F2F) в качестве основного способа оплаты, это не более, чем ознакомительное руководство, чтобы понять, как именно работает Haveno DEX!_

Прежде чем начать чтение этого руководства, **убедитесь, что вы поняли, почему именно децентрализованные биржи (DEX) являются следующим шагом в развитии децентрализованных финансов (DeFi)**. Для полного понимая, советую ознакомиться с [небольшой статьей](https://blog.nihilism.network/servers/finances/index.html) из моего личного блога .

Поскольку мы рассматриваем работу с децентрализованной биржей, **это подразумевает, что мы не рассказываем о том, как использовать веб-сайт, на котором вы покупаете Monero** (как, например, на уже не существующем Localmonero (RIP)), **мы рассматриваем программное обеспечение, которое вы устанавливаете на свой компьютер** (отсюда и берётся термин децентрализация в слове 'DEX'), **для Peer to Peer (P2P) торговли с другими пользователями**.

![03](/img/manuals/haveno-client-f2f/03.png)

Устойчивость Haveno имеет несколько уровней: прежде всего, чем больше узлов в сети, тем сложнее проводить атаку на сеть Haveno. Анонимность, обеспечиваемая технологиями Tor для всех узлов Haveno (которая включена по умолчанию), также повышает общую отказоустойчивость.

Чем больше пиров (пользователей), тем разнообразнее и больше будет конкретная биржа, а значит, и децентрализованный рынок в целом. **Чем быстрее рынок Monero перейдет на использование децентрализованные биржи, тем более неудержимым станет проект в целом**.

И наконец, если сеть Haveno будет сведена на нет (скажем, если все начальные узлы будут каким-то образом удалены), все, что потребуется, - это новый администратор сети, который создаст новую сеть Haveno, поскольку весь код Haveno является открытым, чтобы повторить цикл снова.

Вы можете ознакомиться с моим кратким обзором Haveno DEX на Monerotopia [здесь](https://iv.datura.network/watch?v=hWcwin8bDpc&t=1h9m11s) (обязательно посетите Monerotopia, чтобы узнать самые последние новости из мира Monero, потрясающее шоу!).

## _Начальная настройка_

Во-первых, нам нужно найти сеть Haveno, [Haveno Reto](https://monero.town/post/3143272) появилась одной из первых, как раз её мы и собираемся опробовать:

Давайте возьмем двоичные файлы Haveno из [репозитория Reto на GitHub](https://github.com/retoaccess1/haveno-reto/actions) (который был разветвлен из [исходного репозитория Haveno](https://github.com/haveno-dex/haveno), поддерживаемого Woodser).

[Здесь](https://github.com/retoaccess1/haveno-reto/actions) вы можете просто выбрать последнюю успешную CIсборку, например вот [эту](https://github.com/retoaccess1/haveno-reto/actions/runs/9081479642):

![04](/img/manuals/haveno-client-f2f/04.png)

Затем распакуйте zip-архив:

```
[ mainpc ] [ /dev/pts/5 ] [~]
→ unzip ~/Downloads/HavenoInstaller-ubuntu-latest.zip -d ~/Documents/
Archive:  /home/nihilist/Downloads/HavenoInstaller-ubuntu-latest.zip
  inflating: /home/nihilist/Documents/desktop-1.0.3-SNAPSHOT-all.jar.SHA-256
  inflating: /home/nihilist/Documents/haveno-1.0.3-1.x86_64.rpm
  inflating: /home/nihilist/Documents/haveno_1.0.3-1_amd64.deb

[ mainpc ] [ /dev/pts/5 ] [~]
→ cd ~/Documents/haveno-reto

[ mainpc ] [ /dev/pts/5 ] [~/Documents/haveno-reto]
→ ls
desktop-1.0.3-SNAPSHOT-all.jar.SHA-256  haveno_1.0.3-1_amd64.deb  haveno-1.0.3-1.x86_64.rpm
```

Поскольку в данный момент мы используем Debian, мы будем использовать .deb для установки Haveno, как показано ниже:

```

[ mainpc ] [ /dev/pts/1 ] [~/Documents/haveno-reto]
→ sudo dpkg -i haveno_1.0.3-1_amd64.deb
[sudo] password for nihilist:
Selecting previously unselected package haveno.
(Reading database ... 214512 files and directories currently installed.)
Preparing to unpack haveno_1.0.3-1_amd64.deb ...
Unpacking haveno (1.0.3-1) ...
Setting up haveno (1.0.3-1) ...

#if it fails, run "apt install -f" to install the missing dependencies and then dpkg -i haveno.deb again.
```

Если вы ранее уже использовали Haveno, убедитесь, что вы удалили каталог `haveno` в `~/.local/share/`, чтобы очистить всю предыдущую информацию о вашем кошельке

```
[ mainpc ] [ /dev/pts/1 ] [~/Documents/haveno-reto]
→ rm -rf ~/.local/share/Haveno
```

Если вы хотите увидеть журнал по мере установки haveno, вы можете сделать следующее:

```
[ mainpc ] [ /dev/pts/1 ] [~/Documents/haveno-reto]
→ cd ~/.local/share/Haveno

[ mainpc ] [ /dev/pts/1 ] [.local/share/Haveno]
→ ls
haveno.log  monerod  monero-wallet-rpc  version  xmr_mainnet

[ mainpc ] [ /dev/pts/1 ] [.local/share/Haveno]
→ tail -f haveno.log
May-14 18:22:42.489 [JavaFX Application Thread] INFO  h.n.p.s.p.StoreService: Could not find resourceFile SignedWitnessStore_XMR_MAINNET. That is expected if none is provided yet.
May-14 18:22:42.493 [JavaFX Application Thread] INFO  h.n.p.s.p.HistoricalDataStoreService: We have created the TradeStatistics3Store store for the live data and filled it with 0 entries from the persisted data.
May-14 18:22:42.495 [JavaFX Application Thread] INFO  h.n.p.s.p.StoreService: We copy resource to file: resourceFileName=TradeStatistics3Store_0.0.1_XMR_MAINNET, destinationFile=/home/nihilist/.local/share/Haveno/xmr_mainnet/db/TradeStatistics3Store_0.0.1
May-14 18:22:42.496 [JavaFX Application Thread] INFO  h.n.p.s.p.StoreService: Could not find resourceFile TradeStatistics3Store_0.0.1_XMR_MAINNET. That is expected if none is provided yet.
May-14 18:22:42.513 [JavaFX Application Thread] INFO  h.n.p.s.p.HistoricalDataStoreService: We have created the AccountAgeWitnessStore store for the live data and filled it with 0 entries from the persisted data.
May-14 18:22:42.513 [JavaFX Application Thread] INFO  h.n.p.s.p.StoreService: We copy resource to file: resourceFileName=AccountAgeWitnessStore_0.0.1_XMR_MAINNET, destinationFile=/home/nihilist/.local/share/Haveno/xmr_mainnet/db/AccountAgeWitnessStore_0.0.1
May-14 18:22:42.514 [JavaFX Application Thread] INFO  h.n.p.s.p.StoreService: Could not find resourceFile AccountAgeWitnessStore_0.0.1_XMR_MAINNET. That is expected if none is provided yet.
May-14 18:22:44.518 [JavaFX Application Thread] INFO  h.c.a.HavenoSetup: Init P2P network
May-14 18:22:44.527 [StartTor] INFO  h.n.p.n.NewTor: Starting tor
May-14 18:22:44.527 [JavaFX Application Thread] INFO  h.c.a.HavenoSetup: walletInitialized=false, p2pNetWorkReady=false
```

Затем просто запустите Haveno (он должен быть уже установлен на вашу операционную систему):

![05](/img/manuals/haveno-client-f2f/05.png)

Далее haveno необходимо подключиться к Tor. Если у клиента возникли проблемы с подключением, подождите, пока он не попросит вас произвести настройку Tor вручную:

![06](/img/manuals/haveno-client-f2f/06.png)

Затем  получите ключ Torbridge с веб-сайта torproject.org:

![07](/img/manuals/haveno-client-f2f/07.png)

Вставьте полученный ключ в Haveno и перезапустите его:

![08](/img/manuals/haveno-client-f2f/08.png)

После этого подключение должно работать корректно:

![09](/img/manuals/haveno-client-f2f/09.png)

Возможно, что вам придется немного подождать, пока ваш узел haveno синхронизируется: (1-2 минуты).

![10](/img/manuals/haveno-client-f2f/10.png)

Как только клиент закончит синхронизацию, вы окажетесь 'внутри' Haveno!

![11](/img/manuals/haveno-client-f2f/11.png)

## _Обмен фиатной валюты на Monero, прямая торговля (F2F)_

Первым шагом будет настройка вашего аккаунта для F2F-торговли:

![12](/img/manuals/haveno-client-f2f/12.png)

Здесь мы указываем, что хотим совершить обмен при личной встрече (F2F) в Берлине (Германия), в качестве примера, мы будем использовать фиатную валюту, евро (наличные), вы также можете указать альтернативные способы связи, если вам не нравится встроенный чат в Haveno DEX, например, электронную почту, номер телефона и т.д. Затем нажмите кнопку
`Save new account`:

![13](/img/manuals/haveno-client-f2f/13.png)

Убедитесь, что вы внимательно прочитали, что представляет собой сделка `Обмен при личной встрече`, фиатная валюта -> XMR и каковы ее потенциальные риски, если вы согласны, нажмите `I understand`. Теперь, когда ваш аккаунт создан, перейдите в раздел `Buy`, так как вы хотите купить Monero:

![14](/img/manuals/haveno-client-f2f/14.png)

![15](/img/manuals/haveno-client-f2f/15.png)

Затем вы можете опубликовать торговое предложение `Фиатная валюта -> XMR`, `Обмен при личной встрече`, например, так:

![16](/img/manuals/haveno-client-f2f/16.png)

Итак, мы хотим купить 0.10 XMR по текущей рыночной цене, которая составляет 12 евро. Переходим к следующему шагу:

Здесь мы знакомимся с системой гарантийного депозита [торгового протокола](https://github.com/haveno-dex/haveno/blob/master/docs/trade_protocol/trade-protocol.pdf), который подробно описан в [FAQ по Haveno](https://haveno.exchange/faq/#what-are-the-differences-in-the-trade-protocol). Чтобы объяснить ситуацию, я нарисую простую схему:

```
Цитата из Руководства пользователя Haveno: (https://haveno.exchange/faq/#what-are-the-differences-in-the-trade-protocol)

[...]

Недавно Bisq перешел на протокол 2/2 multisig, а Haveno будет использовать свой протокол - 2/3 multisig. При торговле в разрезе протокола 2/3 multisig каждый трейдер владеет одним ключом; этот ключ сопрягается с ключом другого трейдера и используется для разблокировки средств и депозитов. Это протокол 2 из 3 (2/3), поэтому, для перемещения средств с кошелька с мультиподписью вам нужны только два ключа из трех.

Если все хорошо, оба трейдера используют свои ключи для завершения процесса сделки. Если же что-то пойдет не так, одна из сторон не сможет использовать свой ключ для завершения транзакции, и тут в дело вступает арбитраж.

Арбитраж унаследованы от протокола 2/3 Bisq. Он являются доверенными лицом и обязаны освободить средства одной из двух сторон в случае конфликта. Для этого используется третий ключ протокола из схмы 2/3.

[...]
```

![17](/img/manuals/haveno-client-f2f/17.png)

Проще говоря, в данном случае вы (Боб) хотите лично обменять фиатные деньги на XMR Алисы. **Вы, и Алиса должны внести в сделку некоторое количество Monero в качестве залога**. Это делается для того, чтобы в случае, если вы попытаетесь обмануть Алису, вы что-то потеряли в процессе сделки, что не позволит вам повторно пытаться обмануть других людей, и наоборот.

В связи с тем, что обмен в схеме 2/3 работает с помощью мультиподписи, **для заверешения сделки необходимо наличие как минимум двух сторон**. Если все хорошо, вы и Алиса соглашаетесь на сделку, и залоговый депозит в Monero освобождается. Если нет, то вмешается арбитраж, чтобы наказать нарушителя (не возвращая ему залог) и отдать залог честной стороне.

В следующем примере рассматривается успешная сделка между вами и Алисой. Если вы хотите увидеть торговый спор, ознакомьтесь с этим [руководством](https://blog.nihilism.network/servers/haveno-arbitrator/index.html).

![18](/img/manuals/haveno-client-f2f/18.png)

Здесь вам нужно отправить гарантийный депозит, чтобы иметь возможность опубликовать свое предложение о покупке, просто отправьте его со своего кошелька Мonero, как показано ниже:

![19](/img/manuals/haveno-client-f2f/19.png)

После того, как вы отправили Мonero на торговую площадку Haveno для внесения гарантийного депозита, вам нужно подождать около 20 минут, пока транзакция не будет подтверждена сетью.

![20](/img/manuals/haveno-client-f2f/20.png)

Примерно через 20 минут сделка отображается как активная:

![21](/img/manuals/haveno-client-f2f/21.png)
_Боб: вводит в сделку 0.1005 XMR в качестве гарантийного депозита_

Вы (и другие пользователи Haveno) можете увидеть его на вкладке `Sell`:

![22](/img/manuals/haveno-client-f2f/22.png)

Здесь вам просто нужно дождаться, пока кто-то согласится на сделку. Как только это произойдёт, другому пользователю тоже потребуется отправить свою часть гарантийного депозита, как мы делали раньше. Как только он это сделает, сделка будет отображаться на вашей стороне как инициированная:

![23](/img/manuals/haveno-client-f2f/23.png)
_Алиса вносит в сделку 0.1005 XMR в качестве гарантийного депозита. Затем сделка будет завершена_

Как и раньше, вам нужно подождать, пока сеть подтвердит гарантийный депозит (около 20 минут). Тем временем вы можете пообщаться с трейдером, нажав кнопку `Open Trader Chat`.

![24](/img/manuals/haveno-client-f2f/24.png)

Как только залог будет подтвержден сетью с другой стороны, вы получите уведомление о том, что можно приступить к обмену:

![25](/img/manuals/haveno-client-f2f/25.png)

Следующим шагом вы должны пойти и отдать 12 евро Алисе, после чего вы подтверждаете, что платеж был отправлен:

![26](/img/manuals/haveno-client-f2f/26.png)

Затем дождитесь, пока Алиса подтвердит, что получила 12 евро (это будет отображено как "Пир подтвердил получение средств"):

![27](/img/manuals/haveno-client-f2f/27.png)
_Алиса может отправить 0.10 XMR Бобу, после того как Боб заплатит ей_

Здесь вы просто ждете, пока Monero поступит на ваш кошелек Haveno, они будут отображаться как `В ожидании` в правом верхнем углу:

![28](/img/manuals/haveno-client-f2f/28.png)

Подождите еще 20 минут, пока транзакция будет подтверждена сетью, и она появится в вашем кошельке Haveno как доступный баланс:

![29](/img/manuals/haveno-client-f2f/29.png)
_Торговля завершена успешно, гарантийный депозит освобожден. Боб получил свои 0.1005 XMR обратно, и Алиса тоже. (за вычетом комиссии за транзакцию и комиссии арбитража)_

Вот и все, теперь вы можете пить шампанское, ведь вы совершили свой первый обмен из фиатной валюты в XMR на децентрализованной бирже! 🥂

## _Вывод Monero из Haveno на другой кошелек_

Теперь осталось только вывести Monero из кошелька Haveno на другой кошелек Monero:

![30](/img/manuals/haveno-client-f2f/30.png)

Перейдите в раздел `Funds` > `Send funds`, отметьте опцию `Amounts includes mining fee` и выберите сумму в Monero, которую вы хотите вывести, в данном случае мы выводим всю сумму.

![31](/img/manuals/haveno-client-f2f/31.png)

Затем подтвердите, что вы хотите вывести средства, и проверьте свой кошелек Monero на предмет входящей транзакции:

![32](/img/manuals/haveno-client-f2f/32.png)

И все! Вы только что вывели свои средства на другой кошелек Monero!

---

_Источник: [Haveno DEX Direct Fiat to Monero transactions](https://blog.nihilism.network/servers/haveno-client-f2f/index.html)_

_Перевод: [Mr. Pickles](https://t.me/v1docq47)_  
_Коррекция: [Kukima](https://t.me/Kukima)_
