# Omówienie struktury

```python
❯ ls -l # Wylistowanie wszystkich plików poza ukrytymi
drwxr-xr-x@   - rich 18 Apr 07:23 confs
drwxr-xr-x@   - rich 22 Apr 14:28 docs
drwxr-xr-x@   - rich 22 Apr 09:28 scripts
drwxr-xr-x@   - rich 17 Apr 14:25 sites
.rw-r--r--@ 519 rich 22 Apr 12:23 compose.yaml
.rw-r--r--@ 227 rich 18 Apr 07:24 Dockerfile
```

| Plik         | typ       | Zastosowanie                                    |
| ------------ | --------- | ----------------------------------------------- |
| confs        | Katalog   | Pliki konfiguracyjne domen apache2              |
| docs         | Katalog   | Dokumentacja w formie strony statycznej         |
| scripts      | Katalog   | Skrypty np. powłoki bash                        |
| sites        | Katalog   | Aplikacje firmy OEX CSR                         |
| compose.yaml | Plik yaml | Plik compose do orkiestracji serwisów           |
| Dockerfile   | Plik      | Plik konfiguracyjny domyślnego obrazu kontenera |
