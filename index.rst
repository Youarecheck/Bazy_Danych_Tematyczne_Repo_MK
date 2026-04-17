Konfiguracja bazy danych
========================

Wstęp
-----

Rozdział opisuje uniwersalną konfigurację połączenia z bazą danych w środowiskach dev, test i prod.
Parametry różnią się w zależności od silnika DB (MySQL, PostgreSQL) i środowiska. Poniżej przedstawiono
podejście ogólne, niezależne od technologii.

**Wymagania:**

* dostęp do instancji bazy,
* zmienne środowiskowe,
* narzędzia projektowe.

Podstawowe parametry
--------------------

Każde połączenie wymaga:

* hosta i portu,
* nazwy bazy,
* użytkownika i hasła.

Dane wrażliwe (np. hasła) przechowuj w **zmiennych środowiskowych** – to oddziela konfigurację od kodu.
Pamiętaj: zmienne środowiskowe nie są mechanizmem bezpieczeństwa; w produkcji uzupełnij je o dedykowane
zarządzanie sekretami.

Struktura plików
----------------

Konfiguracja = część wspólna + środowiskowa.

Przykład::

    projekt/
    ├── config/
    │   ├── dev/
    │   ├── prod/
    │   └── shared/
    └── .env.example

Plik ``.env.example`` zawiera tylko przykładowe wartości (bez sekretów).

Środowiska i profile
--------------------

Przełączanie środowisk realizuj przez:

* zmienne środowiskowe,
* profile konfiguracyjne,
* osobne pliki dla dev/test/prod.

Automatyzacja
-------------

Zalecenia:

* walidacja konfiguracji przy starcie,
* skrypty sprawdzające kompletność,
* integracja z CI/CD.

Dokumentację generuj automatycznie (Sphinx).

Przykłady
---------

**MySQL (.env)** ::

    DB_HOST=localhost
    DB_PORT=3306
    DB_NAME=aplikacja
    DB_USER=app_user
    DB_PASSWORD=${DB_PASSWORD}

**PostgreSQL (URL)** ::

    DATABASE_URL=postgresql://user:${DB_PASSWORD}@localhost:5432/aplikacja

Najlepsze praktyki
------------------

#. Nie przechowuj sekretów w repozytorium.
#. Stosuj ``.env.example`` zamiast rzeczywistych danych.
#. Dokumentuj każdy parametr.
#. Używaj spójnych nazw zmiennych.
#. Waliduj konfigurację przed uruchomieniem.

Podsumowanie
------------

Poprawna konfiguracja bazy danych zwiększa bezpieczeństwo i przenośność aplikacji.
