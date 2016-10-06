# API

## Local API

Będąc w tej samej sieci LAN dysponujesz dostępem do lokalnych urządzeń za pośrednictwem lokalnego API. Urządzenia należy wykryć za pośrednictwem protokołu MDNS-SD.

### MDNS-SD Search

Wyszukujemy urządzeń za pośrednictwem zapytania *_light._tcp*.

Przykład zapytania:

```dns-sd -B _light._tcp .```

Przykład wyników:

```BITOAD_SWITCH_9047975
192.168.1.26:80

BITOAD_SWITCH_92824
192.168.1.62:80

BITOAD_SWITCH_92917
192.168.1.56:80 ```

### Local API

Za pośrednictwem 
