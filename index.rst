Konfiguracja bazy danych
=========================

Wstępne kroki
-------------

- Cel rozdziału
- Zakres konfiguracji (środowiska: dev, test, prod)
- Wymagania wstępne


Podstawowe parametry konfiguracyjne
-----------------------------------

- Host, port, nazwa bazy, użytkownik, hasło
- Różnice między systemami baz danych (MySQL, PostgreSQL)
- Zarządzanie sekretami (zmienne środowiskowe)


Struktura plików konfiguracyjnych
---------------------------------

- Główne pliki konfiguracyjne (np. ``.env``, ``config.yaml``)
- Organizacja w projekcie
- Przykład struktury katalogów::

    projekt/
    ├── config/
    │   ├── dev/
    │   ├── prod/
    │   └── shared/
    └── .env.example


Środowiska i profile
--------------------

- Konfiguracja dla development, test, production
- Mechanizm przełączania środowisk
- Wykorzystanie zmiennych środowiskowych


Automatyzacja i narzędzia
-------------------------

- Walidacja konfiguracji (skrypty sprawdzające)
- Generowanie dokumentacji za pomocą Sfina
- Integracja z CI/CD


Przykłady praktyczne
--------------------

Przykład dla MySQL::

   DB_HOST=localhost
   DB_PORT=3306
   DB_NAME=aplikacja
   DB_USER=app_user
   DB_PASSWORD=${DB_PASSWORD}

Przykład dla PostgreSQL::

   DATABASE_URL=postgresql://user:${DB_PASSWORD}@localhost:5432/aplikacja


Najlepsze praktyki
------------------

#. Nigdy nie przechowuj sekretów w plikach wersjonowanych
#. Używaj plików przykładowych (``.env.example``) zamiast rzeczywistych
#. Dokumentuj każdy parametr konfiguracyjny
#. Stosuj spójne nazewnictwo zmiennych
#. Waliduj konfigurację przed uruchomieniem aplikacji


Podsumowanie
------------

- Kluczowe wnioski
- Odnośniki do powiązanej dokumentacji
- Dalsze kroki