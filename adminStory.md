1. Jako administrator, chcę zdalnie aktualizować oprogramowanie biletomatów,
aby zapewnić zgodność z n
2. Jako administrator, chcę mieć dostęp do raportów sprzedaży w czasie 
rzeczywistym, aby monitorować wyniki finansowe. 
3. Jako administrator, chcę konfigurować dostępne bilety, promocje i taryfy w 
systemie centralnym, aby odzwierciedlać zmiany w ofercie. 

## DIAGRAMY PRZYPADKÓW UŻYCIA
### Zarządzanie dostępnością biletów

```mermaid
flowchart TD
    A("Administrator")
    B(["Synchronizacja danych"])
    C(["Powiadomienie o problemach synchronizacji"])
    D(["Zarządzanie dostępnością biletów"])

    A --- D
    B -.-> |Include| D
    D -.-> |Extend| C
```


