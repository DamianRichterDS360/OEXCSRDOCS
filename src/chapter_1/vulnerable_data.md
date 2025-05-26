# Dane wrażliwe

Wszelakie dane wrażliwe (bądź też przekierowania) są zawierane w ukrytym pliku .env.

```bash
❯ tail .env/development/sso_app
SSO_EMAIL_MAIL_SMTP_PORT=<value>

SSO_MAIL_FROM=<value>
SSO_MAIL_FROM_NAME=<value>
SSO_MAIL_SMTP_USER=<value>
SSO_MAIL_SMTP_PASSWORD=<value>
SSO_MAIL_SMTP_HOSTNAME=<value>
SSO_MAIL_SMTP_PORT=<value>
```

Przy starcie kontenera wartości te są przypisywane jako zmienne środowiskowe, które można wykorzystywać już z poziomu interpretera języka PHP.

## Przykłady 

Poniżej znajdują się przykłady zastosowania tych zmiennych środowiskowych.

### Konfiguracja bazy danych

```php
<?php

class DATABASE_CONFIG
{
    public array $default;
    public array $db_rekrutacja;
    public array $db_rozliczenia;
    public array $db_magazyn;

    public function __construct()
    {
        $this->default['driver'] = getenv(name: "SSO_DEFAULT_DB_DRIVER", local_only: true) ?: '';
        $this->default['host'] = getenv(name: "SSO_DEFAULT_DB_HOST", local_only: true) ?: '';
        $this->default['login'] = getenv(name: "SSO_DEFAULT_DB_LOGIN", local_only: true) ?: '';
        $this->default['password'] = getenv(name: "SSO_DEFAULT_DB_PASSWORD", local_only: true) ?: '';
        $this->default['database'] = getenv(name: "SSO_DEFAULT_DB_DATABASE", local_only: true) ?: '';
        $this->default['encoding'] = getenv(name: "SSO_DEFAULT_DB_ENCODING", local_only: true) ?: '';

        $this->db_rekrutacja['driver'] = getenv(name: "SSO_REKRUTACJA_DB_DRIVER", local_only: true) ?: '';
        $this->db_rekrutacja['host'] = getenv(name: "SSO_REKRUTACJA_DB_HOST", local_only: true) ?: '';
        $this->db_rekrutacja['login'] = getenv(name: "SSO_REKRUTACJA_DB_LOGIN", local_only: true) ?: '';
        $this->db_rekrutacja['password'] = getenv(name: "SSO_REKRUTACJA_DB_PASSWORD", local_only: true) ?: '';
        $this->db_rekrutacja['database'] = getenv(name: "SSO_REKRUTACJA_DB_DATABASE", local_only: true) ?: '';
        $this->db_rekrutacja['encoding'] = getenv(name: "SSO_REKRUTACJA_DB_PASSWORD", local_only: true) ?: '';

        $this->db_rozliczenia['driver'] = getenv(name: "SSO_ROZLICZENIA_DB_DRIVER", local_only: true) ?: '';
        $this->db_rozliczenia['host'] = getenv(name: "SSO_ROZLICZENIA_DB_HOST", local_only: true) ?: '';
        $this->db_rozliczenia['login'] = getenv(name: "SSO_ROZLICZENIA_DB_LOGIN", local_only: true) ?: '';
        $this->db_rozliczenia['password'] = getenv(name: "SSO_ROZLICZENIA_DB_PASSWORD", local_only: true) ?: '';
        $this->db_rozliczenia['database'] = getenv(name: "SSO_ROZLICZENIA_DB_DATABASE", local_only: true) ?: '';
        $this->db_rozliczenia['encoding'] = getenv(name: "SSO_ROZLICZENIA_DB_PASSWORD", local_only: true) ?: '';

        $this->db_magazyn['driver'] = getenv(name: "SSO_MAGAZYN_DB_DRIVER", local_only: true) ?: '';
        $this->db_magazyn['host'] = getenv(name: "SSO_MAGAZYN_DB_HOST", local_only: true) ?: '';
        $this->db_magazyn['login'] = getenv(name: "SSO_MAGAZYN_DB_LOGIN", local_only: true) ?: '';
        $this->db_magazyn['password'] = getenv(name: "SSO_MAGAZYN_DB_PASSWORD", local_only: true) ?: '';
        $this->db_magazyn['database'] = getenv(name: "SSO_MAGAZYN_DB_DATABASE", local_only: true) ?: '';
        $this->db_magazyn['encoding'] = getenv(name: "SSO_MAGAZYN_DB_PASSWORD", local_only: true) ?: '';
    }
}
```

### Przekierowania

Produkcyjna wersja aplikacji podczas przekierowywania czy to automatycznego czy też za pomocą hiperlinków sprawdza czy istnieje zmienna środowiskowa.

```php
// Jeżeli istnieje deklaracja zmiennej środowiskowej SSO_WYSYLKI_URL (i nie jest ona pustym łańcuchem znakowym) to używamy jej wartości
// w.p. używamy wartości z produkcyjnej ścieżki (do uzgodnienia)
$params['wysylki_url'] = getenv(name: 'SSO_WYSYLKI_URL', local_only: true) ?: 'https://wysylki121.cursor.pl';
```

Następnie w szablonie pod hiperłącze podstawiana jest ta zmienna (gwarantuje to brak niepożądanego zachowania produkcyjnej wersji).

```html
<a href="<tmpl_var wysylki_url>">
```

### Wyświetlenie wszystkich zmiennych środowiskowych

```bash
task print_all_env_vars:dev
# lub
task penvd
```

<div class="warning">

Powyższe zadanie wymaga zainstalowanej aplikacji jq w celu parsowania konfiguracji dockera.

**Mac OS X**:      brew install jq         <br />
**Debian/Ubuntu**: sudo apt-get install jq <br />
**Fedora**:        sudo dnf install jq     <br />
**Arch**:          sudo pacman -S jq       <br />


</div>

