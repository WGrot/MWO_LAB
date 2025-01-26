1. Jako administrator, chcę zdalnie aktualizować oprogramowanie biletomatów,
aby zapewnić zgodność z n
2. Jako administrator, chcę mieć dostęp do raportów sprzedaży w czasie 
rzeczywistym, aby monitorować wyniki finansowe. 
3. Jako administrator, chcę konfigurować dostępne bilety, promocje i taryfy w 
systemie centralnym, aby odzwierciedlać zmiany w ofercie. 

## DIAGRAMY PRZYPADKÓW UŻYCIA
### Zarządzanie dostępnością biletów

Opis krokowy:
1. Administrator loguje się do panelu zarządzania biletami (Logowanie do
systemu).
2. Administrator wprowadza zmiany w liście dostępnych biletów (np. dodaje nowe
bilety, usuwa nieaktualne) (Edytowanie listy biletów).
3. Administrator zapisuje zmiany w systemie centralnym (Zapisanie listy biletów).
4. System synchronizuje listę biletów z biletomatami i aplikacjami mobilnymi
(Synchronizacja listy biletów).

```mermaid
flowchart TD
    A("Administrator")
    B(["Synchronizacja danych"])
    C(["Powiadomienie o problemach synchronizacji"])
    D(["Zarządzanie dostępnością biletów"])
    E([Logowanie do systemu])
    F([Edytowanie listy biletów])
    H([Zapisanie listy biletów])
    I([Synchronizowanie listy biletów])

    A --- E
    A --- F
    A --- H
    H --- I
    I --- D
    B -.-> |Include| D
    D -.-> |Extend| C
```
### Monitorowanie wyników sprzedaży

Opis krokowy:
1. Administrator loguje się do systemu raportowego (Logowanie do systemu
raportowego).
2. Administrator przegląda raporty sprzedaży w czasie rzeczywistym (Przegląd
raportów sprzedaży).
3. Administrator analizuje dane o sprzedaży na poziomie globalnym i lokalnym
(Analiza danych sprzedaży).
4. Administrator eksportuje raporty do dalszej analizy lub udostępnienia (Eksport
raportów).


```mermaid
flowchart TD
    A("Administrator")
    E([Monitorowanie wyników sprzedaży])
    F([Analiza danych sprzedaży z bazy danych])
    G([Powiadomienie o niezgodnościach w danych sprzedaży])

    A --- E
    F -.-> |Include| E
    G -.-> |Extend| E
```

## Wspólny diagram
```mermaid
flowchart TD
    A("Administrator")
    B(["Synchronizacja danych"])
    C(["Powiadomienie o problemach synchronizacji"])
    D(["Zarządzanie dostępnością biletów"])
    E([Monitorowanie wyników sprzedaży])
    F([Analiza danych sprzedaży z bazy danych])
    G([Powiadomienie o niezgodnościach w danych sprzedaży])

    
    A --- D

    D -.-> |Extend| C
    A --- E
    F -.-> |Include| E
    G -.-> |Extend| E
    B -.-> |Include| D
```


## DIAGRAMY SEKWENCJI
### DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA ZARZĄDZANIE DOSTĘPNOŚCIĄ BILETÓW
- AKTOR: Administrator.
- OBIEKTY: Panelu zarządzania, Serwer aplikacji, Baza danych, Biletomaty i aplikacja.
- KOLEJNOŚĆ KOMUNIKATÓW:
    - Administrator loguje się do panelu zarządzania
    - Panel zarządzania przekazuje dane do serwera.
    - Serwer wysyła zapytanie do bazy danych.
    - Baza odpowiada.
    - Serwer zwraca wynik do panelu zarządzania.
    - Panel zarządzania informuje administratora o sukcesie lub błędzie.
    - Administrator wprowadza zmiany w liście dostępnych biletów.
    - Administrator klika przycisk "zapisz zmiany".
    - Panel zarządzania wysyła dane o zmianach do serwera
    - Serwer wysyła zapytanie do bazy danych o zapisanie zmian
    - Baza danych zwraca sukces albo błąd
    - Serwer wysyła informacje o zmianach do biletomatów i aplikacji
    - biletomaty i aplikacje aktualizują dane o biletach
- SCENARIUSZ ALTERNATYWNY 1 (błędne dane logowania):
    - Administrator loguje się do panelu zarządzania
    - Panel zarządzania przekazuje dane do serwera.
    - Serwer wysyła zapytanie do bazy danych.
    - Baza zwraca informacje o braku dopasowania.
    - Serwer zwraca informacje o błędzie do panelu zarządzania.
    - Panel zarządzania wyświetla komunikat o błędnych danych logowania.
- SCENARIUSZ ALTERNATYWNY 2 (Bład synchronizacji w produkcji)
    - Administrator klika przycisk "zapisz zmiany".
    - Panel zarządzania wysyła dane o zmianach do serwera
    - Serwer wysyła zapytanie do bazy danych o zapisanie zmian
    - Baza danych zwraca sukces albo błąd
    - Serwer wysyła informacje o zmianach do biletomatów i aplikacji
    - biletomaty i aplikacje napotykają bład przy synchronizacji biletów
    - biletomaty i aplikacje informuja serwer o problemie
    - serwer przesyła informacje o błedzie do panelu zarządzania
    - Panel zarządzania wyświetla komunikat o błedzie

```mermaid
sequenceDiagram
PARTICIPANT USER AS ADMINISTRATOR
PARTICIPANT UI AS INTERFEJS ADMINISTRATORA
PARTICIPANT SERWER AS SERWER APLIKACJI
PARTICIPANT DB AS BAZA DANYCH
PARTICIPANT PROD AS BILETOMATY I APLIKACJA MOBILNA


    USER->>UI: wprowadzenie danych logowania
    UI->>SERWER: przesłanie danych logowania
    SERWER->>DB: weryfikacja danych
    ALT DANE POPRAWNE
    DB-->>SERWER: wynik pozytywny
    SERWER-->>UI: Potwierdzenie logowania
    UI-->>USER: Wyświetlenie ekranu głównego
    ELSE DANE BŁĘDNE
    DB-->>SERWER: Dane niepoprawne
    SERWER-->>UI: Informacja o błędzie
    UI-->>USER: Wyświetlenie komunikatu o błędzie
    END


    USER->>UI: Wprowadź zmiany w liście dostępnych biletów
    UI -->> USER: 
    USER->>UI: Zapisz zmiany
    UI ->> SERWER: Zmieniono dostępność biletów
    SERWER ->> DB: Zapisz zmiany 
    DB -->> SERWER: Sukces
    SERWER ->> PROD: Synchronizuj listę biletów
    ALT SUKCES SYNCHRONIZACJI
    PROD -->> SERWER: Synchronizacja powiodła się
    SERWER -->> UI: informacja o zapisaniu zmian
    UI -->> USER: Wyświetl komunikat o zapisaniu zmian
    ELSE BŁĄD SYNCHRONIZACJI
    PROD -->> SERWER: Bład podczas synchronizacji
    SERWER -->> UI: Informacja o błedzie
    UI -->> USER: Wyświetl komunikat o błędzie
    END
```
    
