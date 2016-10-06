# API

## Local API

Będąc w tej samej sieci LAN dysponujesz dostępem do lokalnych urządzeń za pośrednictwem lokalnego API. Urządzenia należy wykryć za pośrednictwem protokołu MDNS-SD.

### MDNS-SD Search

Wyszukujemy urządzeń za pośrednictwem zapytania *_light._tcp*.
Aktualny port zwracany jest przez MDNS-SD (domyślnie jest to port 80).

Przykład zapytania:

```dns-sd -B _light._tcp .```

Przykład wyników:

```Browsing for _light._tcp
DATE: ---Thu 06 Oct 2016---
12:17:18.846  ...STARTING...
Timestamp     A/R    Flags  if Domain               Service Type         Instance Name
12:17:18.857  Add        3   5 local.               _light._tcp.         BITOAD_SWITCH_9047975
12:17:18.857  Add        3   5 local.               _light._tcp.         BITOAD_SWITCH_92917
12:17:18.857  Add        2   5 local.               _light._tcp.         BITOAD_SWITCH_92824
```

### Local API

Za pośrednictwem 
