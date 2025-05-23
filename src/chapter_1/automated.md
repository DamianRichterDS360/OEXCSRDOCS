# Zautomatyzowany start

## Taskfile

Powyższe zadania mogą być zautomatyzowane poprzez wykorzystanie programu [Taskfile](https://taskfile.dev/).

Po instalacji programu użytkownik powinien upewnić się czy program został aby na pewno poprawnie zainstalowany. Przykładowo uruchomienie terminala i wpisanie komendy `task --version` powinno pokazać obecną wersję programu:

```bash
❯ task --version
3.43.3
```

Katalog główny projektu zawiera plik `Taskfile.yml`, który to z kolei zawiera definicje zadań jakie mogą zostać wykonane. Przyjrzyjmy się jednemu z tych zadań:

```yaml
tasks:
    default:
       cmds:
           - task: run:dev
```

Powyższe zadanie jest zadaniem głównym tzn. uruchamia się ono po wpisaniu komendy `task` składa się z innego zadania `run:dev`, rzućmy na nie okiem.

Upewniwszy się, że jesteśmy w katalogu głównym aplikacji lub też w dowolnym katalogu 'dziecku' projektu. Spróbujmy uruchomić główne zadanie.

```bash
❯ task
Script require root access in order to append hosts to /ect/hosts
Backup is created in same directory just in case if something bad happened!
127.0.0.1 aplikacje121.local was not found in your /etc/hosts
::1 aplikacje121.local was not found in your /etc/hosts
127.0.0.1 wysylki121.local was not found in your /etc/hosts
::1 wysylki121.local was not found in your /etc/hosts
127.0.0.1 wysylki121test.local was not found in your /etc/hosts
::1 wysylki121test.local was not found in your /etc/hosts
127.0.0.1 eventyb.local was not found in your /etc/hosts
::1 eventyb.local was not found in your /etc/hosts
127.0.0.1 filesbat.local was not found in your /etc/hosts
::1 filesbat.local was not found in your /etc/hosts
127.0.0.1 rekrutacje121.local was not found in your /etc/hosts
::1 rekrutacje121.local was not found in your /etc/hosts
127.0.0.1 rozliczeniabat.local was not found in your /etc/hosts
::1 rozliczeniabat.local was not found in your /etc/hosts
.
.
.
task: [run:dev] docker compose up -d
[+] Running 12/12
 ✔ Network cursor_default                 Created                                                                                                                     0.0s
 ✔ Container cursor-web-1                 Started                                                                                                                     0.5s
 ✔ Container cursor-rozliczenia_db-1      Started                                                                                                                     0.3s
 ✔ Container cursor-files_bat_db-1        Started                                                                                                                     0.5s
 ✔ Container cursor-magazyn_db-1          Started                                                                                                                     0.5s
 ✔ Container cursor-eventy_db-1           Started                                                                                                                     0.6s
 ✔ Container cursor-rekrutacja_db-1       Started                                                                                                                     0.3s
 ✔ Container cursor-sso_db-1              Started                                                                                                                     0.3s
 ✔ Container cursor-horeca_db-1           Started                                                                                                                     0.6s
 ✔ Container cursor-mailcrab-1            Started                                                                                                                     0.6s
 ✔ Container cursor-rekrutacja_test_db-1  Started                                                                                                                     0.5s
 ✔ Container cursor-magazyn_test_db-1     Started                                                                                                                     0.3s
```

Zadanie wpierw uruchamia skrypt dodający fałszywe domeny na sam koniec powinien powiadomić nas o uruchomieniu komendy.

Nazwy hostów znajdują się w pliku taskfile spróbuj udać się na jedną ze stron znajdujących się w pliku taskfile.

```yaml
# Taskfile.yaml
HOSTS: [
    aplikacje121.local,
    wysylki121.local,
    wysylki121test.local,
    eventyb.local,
    filesbat.local,
    rekrutacje121.local,
    rozliczeniabat.local,
    rozliczeniatest.local,
    zamowieniabat.local,
    zamowieniabattest.local,
]
```

Poprzez wpisanie jednego z adresów np. http://rozliczeniabat.local

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

