# Start

Do uruchomienia aplikacji potrzebne są następujące narzędzia:
- Docker
- Docker compose

Następnie z poziomu katalogu w którym znajduje się plik `compose.yml` aplikacja może zostać uruchomiona za pomocą poniższej komendy:

```bash
docker compose up
```

Poprawne uruchomienie aplikacji powinno wyświetlić następujące informacje:

```bash
[+] Running 2/2
 ✔ Network oexcursor_default  Created                                                                         0.1s
 ✔ Container oexcursor-web-1  Created                                                                         0.1s
Attaching to web-1
web-1  | [Tue Apr 22 12:22:41.212074 2025] [mpm_prefork:notice] [pid 7] AH00163: Apache/2.4.41 (Ubuntu) PHP/8.1.32 configured -- resuming normal operations
```

Plik `/etc/hosts` powinien zawierać translację ścieżek w następującej postaci:

```python
───────┬───────────────────────────────────────────────────────────────────────
       │ File: /etc/hosts
───────┼───────────────────────────────────────────────────────────────────────
   1   │ ##
   2   │ # Host Database
   3   │ #
   4   │ # localhost is used to configure the loopback interface
   5   │ # when the system is booting.  Do not change this entry.
   6   │ ##
   7   │ 127.0.0.1       localhost
   8   │ 255.255.255.255 broadcasthost
   9   │ ::1             localhost
  10   │
  11   │
  12   │ 127.0.0.1       aplikacje121.local
  13   │ 127.0.0.1       wysylki121.local
  14   │
  15   │ ::1    aplikacje121.local
  16   │ ::1    wysylki121.local
  17   │
───────┴───────────────────────────────────────────────────────────────────────
```

Następnie kierując się za pomocą przeglądarki internetowej na adres np. `http://aplikacje121.local`. Powinniśmy zobaczyć stronę głowną tej aplikacji.

## Omówienie struktury

```python
❯ ls -l # Wylistowanie wszystkich plików poza ukrytymi
drwxr-xr-x@   - rich 18 Apr 07:23 confs
drwxr-xr-x@   - rich 22 Apr 14:28 docs
drwxr-xr-x@   - rich 22 Apr 09:28 scripts
drwxr-xr-x@   - rich 17 Apr 14:25 sites
.rw-r--r--@ 519 rich 22 Apr 12:23 compose.yaml
.rw-r--r--@ 227 rich 18 Apr 07:24 Dockerfile
```

| Plik | typ | Zastosowanie |
| --------------- | --------------- | --------------- |
| confs | Katalog | Pliki konfiguracyjne domen apache2 |
| docs | Katalog | Dokumentacja w formie strony statycznej |
| scripts | Katalog | Skrypty np. powłoki bash |
| sites | Katalog | Aplikacje firmy OEX CSR |
| compose.yaml | Plik yaml | Plik compose do orkiestracji serwisów |
| Dockerfile | Plik | Plik konfiguracyjny domyślnego obrazu kontenera |

