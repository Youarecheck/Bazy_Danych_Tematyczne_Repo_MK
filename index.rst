Konfiguracja bazy danych PostgreSQL
===================================

Wstęp
-----

Rozdział opisuje konfigurację połączenia z bazą danych **PostgreSQL** – dojrzałym, open-source'owym systemem zarządzania relacyjnymi bazami danych (RDBMS), który wyróżnia się pełną zgodnością ze standardem SQL oraz silnym naciskiem na rozszerzalność i niezawodność.

**Wymagania:**

* dostęp do instancji PostgreSQL,
* zmienne środowiskowe,
* narzędzia projektowe (np. ``psql``, ``pg_isready``).

Podstawowe parametry
--------------------

Każde połączenie z PostgreSQL wymaga:

* hosta i portu (domyślnie ``localhost`` i ``5432``),
* nazwy bazy danych,
* użytkownika i hasła.

Dane wrażliwe (np. hasła) przechowuj w **zmiennych środowiskowych** – to oddziela konfigurację od kodu.
Pamiętaj: zmienne środowiskowe nie są mechanizmem bezpieczeństwa; w produkcji uzupełnij je o dedykowane
zarządzanie sekretami (np. HashiCorp Vault, AWS Secrets Manager).

Lokalizacja i struktura katalogów PostgreSQL
--------------------------------------------

Pliki konfiguracyjne PostgreSQL znajdują się w katalogu danych (``PGDATA``).

**Główne pliki konfiguracyjne:**

* ``postgresql.conf`` – podstawowa konfiguracja serwera (port, pamięć, logi)
* ``pg_hba.conf`` – kontrola dostępu (kto i skąd może się łączyć)
* ``pg_ident.conf`` – mapowanie użytkowników systemowych na role PostgreSQL

**Domyślne lokalizacje:**

| System | Katalog danych |
|--------|----------------|
| Linux (RPM/DEB) | ``/var/lib/pgsql/data/`` lub ``/var/lib/postgresql/*/main/`` |
| Linux (kompilacja źródłowa) | ``/usr/local/pgsql/data/`` |
| Windows | ``C:\Program Files\PostgreSQL\<version>\data\`` |
| Docker | ``/var/lib/postgresql/data/`` (w kontenerze) |

**Zmiana katalogu danych:**

.. code-block:: bash

    # Inicjalizacja nowego katalogu
    initdb -D /ścieżka/do/katalogu

    # Uruchomienie z niestandardowym PGDATA
    pg_ctl -D /ścieżka/do/katalogu start

**Struktura katalogu danych:**

::

    $PGDATA/
    ├── postgresql.conf   # główny plik konfiguracyjny
    ├── pg_hba.conf       # polityka dostępu
    ├── pg_ident.conf     # mapowanie tożsamości
    ├── base/             # rzeczywiste bazy danych
    ├── global/           # tabele systemowe
    ├── log/              # logi serwera
    └── pg_wal/           # Write-Ahead Log (WAL)

**Sprawdzenie aktualnej lokalizacji:**

.. code-block:: sql

    SHOW data_directory;
    SHOW config_file;
    SHOW hba_file;

Środowiska i profile
--------------------

Przełączanie środowisk PostgreSQL (dev/test/prod) realizuj przez:

* zmienne środowiskowe (np. ``PGHOST``, ``PGPORT``, ``PGDATABASE``, ``PGUSER``, ``PGPASSWORD``),
* profile konfiguracyjne,
* osobne pliki ``.env`` dla każdego środowiska.

Automatyzacja
-------------

Zalecenia dla PostgreSQL:

* walidacja konfiguracji przy starcie za pomocą ``pg_isready``,
* skrypty sprawdzające kompletność zmiennych środowiskowych,
* integracja z CI/CD (np. GitHub Actions, GitLab CI).

Dokumentację generuj automatycznie (Sphinx).

Przykłady
---------

**PostgreSQL – zmienne środowiskowe (.env)** ::

    PGHOST=localhost
    PGPORT=5432
    PGDATABASE=aplikacja
    PGUSER=app_user
    PGPASSWORD=${DB_PASSWORD}

**PostgreSQL – URL połączenia** ::

    DATABASE_URL=postgresql://${PGUSER}:${PGPASSWORD}@${PGHOST}:${PGPORT}/${PGDATABASE}

**PostgreSQL – sprawdzenie połączenia** ::

    pg_isready -h ${PGHOST} -p ${PGPORT} -U ${PGUSER} -d ${PGDATABASE}

Najlepsze praktyki
------------------

#. Nie przechowuj sekretów w repozytorium.
#. Stosuj ``.env.example`` zamiast rzeczywistych danych.
#. Używaj standardowych zmiennych środowiskowych PostgreSQL (``PG*``).
#. Waliduj konfigurację przed uruchomieniem (``pg_isready``).
#. W środowiskach produkcyjnych używaj połączeń SSL/TLS (``sslmode=require``).

Podsumowanie
------------

Poprawna konfiguracja połączenia z PostgreSQL zwiększa bezpieczeństwo i przenośność aplikacji między środowiskami. Standardowe zmienne środowiskowe oraz narzędzia takie jak ``pg_isready`` ułatwiają automatyzację i walidację.