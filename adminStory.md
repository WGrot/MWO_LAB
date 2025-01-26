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
    X([Monitorowanie wyników sprzedaży])
    Y([Analiza danych sprzedaży])
    Z([Powiadomienie o niezgodnościach w danych sprzedaży])
    O([Powiadomienie o błędach danych])
    M([Logowanie do systemu raportowego])
    N([Przegląd raportów sprzedaży])
    P([Eksport raportów])


    A --- X
    A --- Z
    A --- M
    A --- P
    A --- N
    X -.-> |Include| Y
    O -.-> |Extend| Z
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
### Monitorowanie wyników sprzedaży
- AKTOR: Administrator.
- OBIEKTY: SYSTEM RAPORTOWY, SERWER APLIKACJI
- KOLEJNOŚĆ KOMUNIKATÓW:
    - Administrator loguje się do systemu raportowego
    - System raportowy pobiera dostępne raport.
    - Serwer aplikacji zwraca listę dostępnych raportów.
    - Administrator analizuje dane o sprzedaży na poziomie globalnym i lokalnym 
    - Administrator eksportuje raporty do dalszej analizy 
    - System raportowy do eksportuje raporty serwera aplikacji
- SCENARIUSZ ALTERNATYWNY 1 (Brak dostępnych raportów ):
    - Serwer aplikacji zwraca błąd
    - System raportowy wyświetla komunikat błędu

```mermaid
sequenceDiagram
PARTICIPANT ADMIN AS ADMINISTRATOR
PARTICIPANT UI AS SISYSTEM RAPORTOWY
PARTICIPANT SERWER AS SERWER APLIKACJI

ADMIN ->> UI: Logowanie do systemu raportowego
UI ->> SERWER: Pobierz raporty
alt Raporty dostępne
    SERWER ->> UI: Lista Raportów
    UI ->> ADMIN: 
    ADMIN ->> ADMIN: Analizuj rapoty
    ADMIN ->> UI: Eksportuj raporty
    UI ->> SERWER: Eksportuj raporty
    SERWER ->> UI: 
    UI ->> ADMIN: 
else Brak dostępnych raportów 
    SERWER ->> UI: Błąd
    UI ->> UI: Wyświetl błąd
    UI ->> ADMIN: 
end
```
