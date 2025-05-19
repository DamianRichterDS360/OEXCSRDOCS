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


## Automatyzacja

Powyższe zadania mogą być zautomatyzowane poprzez wykorzystanie programu [Taskfile](https://taskfile.dev/). Po instalacji programu użytkownik powinien upewnić się czy program znajduję się w ścieżce zmiennej środowiskowej PATH!

Katalog główny projektu zawiera plik `Taskfile.yml`, który to z kolei zawiera definicje zadań jakie mogą zostać wykonane. Przyjrzyjmy się jednemu z tych zadań:

```yaml
tasks:
    default:
       cmds:
           - task: run:dev
```

Powyższe zadanie jest zadaniem głównym tzn. uruchamia się ono po wpisaniu komendy `task` składa się z innego zadania `run:dev`, rzućmy na nie okiem.

### Uruchamianie aplikacji w trybie deweloperskim

```yaml
run:dev:
  desc: Runs containers for development
  aliases: [rd]
  cmds:
    - task: configure:hosts:dev:delete
    - task: configure:hosts:dev:add
    - docker compose up -d {{.CLI_ARGS}}
```

### Uruchamianie aplikacji w trybie deweloperskim

definicja `run:dev` zawiera:
- opis zadania: Uruchamianie kontenerów dla trybu deweloperskiego
- 2 zadania configure:hosts:dev do usuwania hostów i ich dodawania
- komende docker compose uruchamiającą kontenery w trybie `detached` czyli kontenery nie będą pracować w głównej sesji powłoki (prościej mówiąc nie ujrzymy domyślnie ich logów).

### Zatrzymywanie  aplikacji w trybie deweloperskim

```yaml
stop:dev:
    desc: Stops containers for development if they are already running
    aliases: [sd]
    vars:
      CONTAINER: 'cursor-web-1'
    preconditions:
      - docker ps --filter "name={{.CONTAINER}}" | grep {{.CONTAINER}}
    cmds:
      - task: configure:hosts:dev:delete
      - docker compose down
```

```bash
# Zatrzymywanie kontenerów
task stop:dev
# Można także użyć skrótu (alias)
task sd
```

### Przebudowywanie kontenerów

Jeżeli do pliku `Dockerfile` naniesiono jakieś zmiany, należy dokonać przebudowy obrazu. Aby uruchomić aplikację wraz z przebudową obrazu użyj poniższej komendy:

```bash
# Forwardowanie flag do komendy '{{.CLI_ARGS}}'
task rd -- --build
```

