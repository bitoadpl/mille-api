# Mille API

Z urządzenia Mille Switch można korzystać przy pomocy czterech interefejsów:

* lokalne REST API + webhooks dostępne w sieci LAN, w której operują urządzenia
* publiczne REST API + OAUTH + webhooks chmurowe dostępne za pośrednictwem Internetu
* interfejs MQTT dostępny w chmurze
* interfejs IFTTT

## Local API

Będąc w tej samej sieci LAN dysponujesz dostępem do lokalnych urządzeń za pośrednictwem lokalnego API. Urządzenia należy wykryć za pośrednictwem protokołu MDNS-SD lub w wypadku połączenia z urządzeniem w trybie Access Pointa skorzystać z domyślnego adresu 192.168.2.1:80

### MDNS-SD Search

Wyszukujemy urządzeń za pośrednictwem zapytania *_light._tcp*.
Aktualny port zwracany jest przez MDNS-SD (domyślnie jest to port 80).

Przykład zapytania:

```
dns-sd -B _light._tcp .
```

Przykład wyników:

```
Browsing for _light._tcp
DATE: ---Thu 06 Oct 2016---
12:17:18.846  ...STARTING...
Timestamp     A/R    Flags  if Domain               Service Type         Instance Name
12:17:18.857  Add        3   5 local.               _light._tcp.         BITOAD_SWITCH_9047975
12:17:18.857  Add        3   5 local.               _light._tcp.         BITOAD_SWITCH_92917
12:17:18.857  Add        2   5 local.               _light._tcp.         BITOAD_SWITCH_92824
```

Przykład zapytania:

```
dns-sd -L BITOAD_SWITCH_92824 _light._tcp .
```

Przykład wyników:

```
Lookup BITOAD_SWITCH_92824._light._tcp.local
DATE: ---Thu 06 Oct 2016---
12:19:49.452  ...STARTING...
12:19:49.812  BITOAD_SWITCH_92824._light._tcp.local. can be reached at bitoad_switch_92824.local.:80 (interface 5)
```

Przykład zapytania:

```
dns-sd -G v4 BITOAD_SWITCH_92824.local
```

Przykład wyników

```
DATE: ---Thu 06 Oct 2016---
13:00:49.035  ...STARTING...
Timestamp     A/R Flags if Hostname                               Address                                      TTL
13:00:49.327  Add     2  5 bitoad_switch_92824.local.             192.168.1.62                                 120
```

### Local API

Znając IP i port lokalnego API można za pośrednictwem zapytań REST wywoływać komendy

#### GET /on

Zapala światło (moc=100)

Request:

```
curl http://192.168.1.100:80/on
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

#### GET /off

Gasi światło (moc=0)

Request:

```
curl http://192.168.1.100:80/off
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

#### GET /toggle

Naprzemiennie gasi (moc=0) i zapala światło (moc=100).
Przy stanie przejściowym (moc &gt; 0 i moc &lt; 100) domyślnie gasi.

Request:

```
curl http://192.168.1.100:80/toggle
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

#### GET /state

Pobiera aktualny stan urządzenia.

Request:

```
curl http://192.168.1.100:80/state
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

Zwracana jest wartośc liczbowa od 0 do 100. Gdzie 0 oznacza lampę zgaszoną a 100 świecącą z pełną mocą.

#### (Deprecated) GET /config?ssid=xxx&password=xxx

Nakazuje urządzeniu przełączyć się do sieci wifi. Po wywołaniu API przestanie być dostępne ze względu na dokonywanie połączenia. W razie sukcesu urządzenie pojawi się w nowej sieci LAN. W razie porażki otworzy własną siec wifi (tryb Access Point) gdzie będzie oczekiwało na połączenia.

Request:

```
curl http://192.168.1.100:80/config?ssid=xxx&password=xxx
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

#### (planned) POST /config

Przesłanie konfiguracji do połączenia do sieci wifi i chmurowego serwera MQTT.

```
tbd
```

#### (planned) GET /webhook/[shortpress|longpress]

Pobranie aktualnie zdefiniowananych adresów dla webhook

Request:

```
curl http://192.168.1.100:80/webhook/shortpress
curl http://192.168.1.100:80/webhook/longpress
```

Kody HTTP:

```
200 - SUKCES
404 - Brak zdefiniowanego webhooka
inny - BŁĄD
```

Response (dla 200):

```
"http://somedomain/someaddress"
```

#### (planned) POST /webhook/[shortpress|longpress]

Ustalenie adresu dla webhooka po zarejestrowaniu shortpress i longpress. Należy definiować je z ostrożnością i kierować do zasobów, które są w stanie szybko obsłużyć przychodzące zapytanie. W razie zwłoki połączenie zostanie przerwane i nie będzie wznawiane. Preferowane jest użycie adresów IP zamiast nazw domenowych. Urządzenie natychmiast po wysłaniu stosownego zapytania HTTP przerywa połączenie nie czekając na odpowiedź. Upewnij się, że proces po stronie twojego serwera potrafi obsłużyć taką sytuację (w szczególności nie zostanie zakończony w razie zamknięcia połączenia).

Request:

```
tbd
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

#### (planned) DELETE /webhook/[shortpress|longpress]

Usunięcie zdefiniowanego webhooka

Request:

```
curl -X DELETE http://192.168.1.100:80/webhook/shortpress
curl -X DELETE http://192.168.1.100:80/webhook/longpress
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

#### (planned) GET /label

Pobranie aktualnie zdefiniowanej lokalnej nazwy urządzenia.

Request:

```
curl -X http://192.168.1.100:80/label
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```
Zwrócony zostanie string z nazwą

#### (planned) POST /label

Ustalenie nazwy urządzenia. Nazwa powinna być legalną nazwą domenową.

Request:

```
tbd
```

Kody HTTP:

```
200 - SUKCES
400 - Nieprawidłowa nazwa
inny - BŁĄD
```

#### (planned) DELETE /label

Przywrócenie domyślnej nazwy urządzenia

```
curl -X DELETE http://192.168.1.100:80/label
```

Kody HTTP:

```
200 - SUKCES
inny - BŁĄD
```

Zwrócony zostanie string z domyślną nazwą

## Public API

Jeżeli chcesz skorzystać z publicznego API prosimy o kontakt z services@bitoad.com

## Public MQTT Access

Jeżeli chcesz skorzystać z dostępu za pośrednictwem MQTT prosimy o kontakt z services@bitoad.com

## IFTTT

Usługa IFTTT zapewnia komunikację z ponad setką typów urządzeń i usług za pośrednictwem kanału Maker (https://ifttt.com/maker). Jeżeli chcesz skorzystać z dostępu za pośrednictwem IFTTT prosimy o kontakt z services@bitoad.com
