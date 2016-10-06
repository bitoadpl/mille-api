# API

Z urządzenia można korzystać przy pomocy dwóch interefejsów. Pierwszy to lokalne API dostępne w sieci LAN, w której operują urządzenia. Drugie to API chmurowe, dostępne za pośrednictwem Internetu.

## Local API

Będąc w tej samej sieci LAN dysponujesz dostępem do lokalnych urządzeń za pośrednictwem lokalnego API. Urządzenia należy wykryć za pośrednictwem protokołu MDNS-SD.

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
### Local API

Znając IP i port lokalnego API można za pośrednictwem zapytań REST wywoływać komendy

#### GET /on

#### GET /off

#### GET /toggle

#### GET /state

#### (Deprecated) GET /configure?ssid=xxx&password=xxx

#### POST /configure



