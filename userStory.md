1. Jako użytkownik, chcę płacić za bilet kartą, gotówką lub telefonem, aby mieć
większą elastyczność w wyborze metody płatności.
2. Jako użytkownik, chcę otrzymać wyraźne instrukcje na ekranie, aby wiedzieć,
jak dokonać zakupu krok po kroku.
3. Jako użytkownik, chcę widzieć czas pozostały na decyzję (np. wyświetlany
licznik czasu), aby móc szybko podjąć działanie.
4. Jako użytkownik, chcę szybko wybrać rodzaj biletu, aby zminimalizować czas 
spędzony przy biletomacie. 
5. Jako użytkownik, chcę mieć możliwość wyboru języka, aby móc korzystać z 
biletomatu bez względu na znajomość języka lokalnego. 
6. Jako użytkownik, chcę sprawdzić poprawność transakcji przed jej finalizacją, 
aby uniknąć pomyłek. 
7. Jako użytkownik, chcę otrzymać potwierdzenie zakupu (np. wydruk biletu lub 
elektroniczny bilet), aby móc korzystać z transportu zgodnie z przepisami. 


## Diagram przypadków użycia
### 2. Wybór języka
Opis krokowy:
1. Użytkownik uruchamia biletomat (Rozpoczęcie interakcji).
2. System wyświetla ekran powitalny z opcjami wyboru języka (Wyświetlenie opcji 
języka).
3. Użytkownik wybiera preferowany język (Wybór języka).
4. System dostosowuje interfejs do wybranego języka (Dostosowanie interfejsu).
5. Użytkownik w dowolnym momencie może anulować proces (Anulowanie 
transakcji).
Relacje:
• Include: Ustawienie domyślnego języka (Domyślny język).
• Include: Anulowanie transakcji (Anulowanie transakcji).
• Extend: Wyświetlenie listy popularnych języków (opcjonalne) (Lista 
popularnych języków)

```mermaid
flowchart TD
    A[Użytkownik]
    D([Anulowanie transakcji])  
    F(["Wybór języka"])
    G(["Domyślny język"])
    H(["Lista popularnych języków"])
    I(["Rozpoczęcie interakcji"])
    J([Wyświetlenie opcji języka])
    K([Dostosowanie interfejsu])

   
    A --> I
    I --> J
    I --> K
    I -.-> |Include|G
    H -.-> |Extend|F
    I -.-> |Include|D
    J ---> F

```

### Płatność za bilet
Opis krokowy:
1. Użytkownik wybiera metodę płatności (karta, gotówka, telefon) (Wybór metody
płatności).
2. System weryfikuje dostępność wybranej metody (Weryfikacja metody
płatności).
3. Użytkownik dokonuje płatności (np. wprowadza kartę, gotówkę, korzysta z NFC)
(Realizacja płatności).
4. System potwierdza zakończenie transakcji (Potwierdzenie transakcji).
5. Użytkownik w dowolnym momencie może anulować proces (Anulowanie
transakcji).


```mermaid
flowchart TD
    A[Użytkownik]
    B([Płatność za bilet])
    C([Weryfikacja metody płatnośći])
    D([Anulowanie transakcji])
    E([Obsługa błędów płatnośći])
    F([Wybór metody płatności])
    I([Zrealizowanie płatności])
    J([Potwierdzenie transakcji])


    A --> B
    A --> F
    A --> I
    A --> J
    F -.-> |Include| C
    B -.-> |Include| D
    E -.-> |extends| B
```

### Wspólny diagram
```mermaid
flowchart TD
    A[Użytkownik]
    B([Płatność za bilet])
    C([Weryfikacja metody płatnośći])
    D([Anulowanie transakcji])
    E([Obsługa błędów płatnośći])
    F([Wybór metody płatności])
    I([Zrealizowanie płatności])
    J([Potwierdzenie transakcji])
    X(["Wybór języka"])
    Y(["Domyślny język"])
    Z(["Lista popularnych języków"])
    V(["Rozpoczęcie interakcji"])
    P([Wyświetlenie opcji języka])
    O([Dostosowanie interfejsu])

    A --> B
    A --> F
    A --> I
    A --> J
    F -.-> |Include| C
    B -.-> |Include| D
    E -.-> |extends| B


   
    A --> V
    V --> P
    V --> O
    V -.-> |Include|Y
    Z -.-> |Extend|X
    V -.-> |Include|D
    P ---> X

 
```

## DIAGRAMY SEKWENCJI
### DIAGRAM SEKWENCJI DLA PRZYPADKU UŻYCIA ZMIANY JĘZYKA
- AKTOR: Użytkownik.
- OBIEKTY: Interfejs użytkownika, Serwer aplikacji, Baza danych.
- KOLEJNOŚĆ KOMUNIKATÓW:
    - Użytkownik uruchamia okno zmiany języka
    - Interfejs przekazuje informacje do serwera
    - Serwer wysyła zapytanie o dostępne języki do bazy danych.
    - Baza danych zwraca listę dostępnych języków.
    - Serwer wysyła informacje o dostępnych językach do interfejsu użytkownika
    - Interfejs użytkownika wyświetla listę dostępnych języków
    - Użytkownik wybiera jeden z dostępnych języków
    - Interfejs użytkownika ustawia wybrany język
- SCENARIUSZ ALTERNATYWNY 1 (Użytkownik anulował zmianę języka):
    - Użytkonik wybiera opcję anuluj.
    - Interfejs użytkownika zamyka okno zmiany języka.
    - Interfejs użytkownika wraca do okna głównego

 ```mermaid
sequenceDiagram
PARTICIPANT USER AS UŻYTKOWNIK
PARTICIPANT UI AS INTERFEJS UŻYTKOWNIKA
PARTICIPANT SERWER AS SERWER APLIKACJI
PARTICIPANT DB AS BAZA DANYCH


    USER->>UI: Pokaż okno zmiany języków
    UI ->> SERWER: Wybrano okno zmiany języka
    SERWER ->> DB: Pobierz listę języków
    DB -->> SERWER: Lista języków
    SERWER -->> UI: Przekaż listę języków
    UI -->> USER: Wyświetl dostępne języki
    ALT Zmieniono język
        USER ->> UI: Wybierz jeden z języków
        UI ->> UI: Zmień język interfejsu
        UI -->> USER: 
    ELSE Anulowano
        USER ->> UI: Anuluj zmianę języka
        UI ->> UI: Zamknij okno zmiany języka
        UI -->> USER: Wróc do okna głównego
    END

 
```

### DIAGRAM SEKWENCJI DLA PRZYPADKU PŁATNOŚĆ ZA BILET
- AKTOR: UŻYTKOWNIK.
- OBIEKTY: BILETOMAT SYSTEM BANKOWY.
- KOLEJNOŚĆ KOMUNIKATÓW:
    - Użytkownik wybiera metodę płatności.
    - Biletomat weryfikacje dostępność wybranej metody.
    - System Bankowy zwraca dostępność metody płatności
    - Użytkownik dokonuje płatności.
    - Biletomat wykonuje płatność.
    - System Bankowy potwierdzenie transakcji.
    - Biletomat wyświetla potwierdzenie zakończenia transakcji.
    - Biletomat wydaje bilety.
    - Użytkownik odbiera bilety.

- SCENARIUSZ ALTERNATYWNY 1 (wybrana metoda jest nie dostępna):
    - Użytkownik wybiera metodę płatności.
    - System Biletomat weryfikuje dostępność wybranej metody.
    - System Bankowy zwraca błąd.
    - Biletomat wyświetla błąd niedostępności medoty płatności.

- SCENARIUSZ ALTERNATYWNY 2 (Brak środków):
    - Użytkownik dokonuje płatności.
    - Biletomat wykonuje płatność.
    - System Bankowy zwraca błąd płatnoći.
    - Biletomat wyświetla błąd "brak środków". 

```mermaid
sequenceDiagram
    Użytkownik->>Biletomat: Wybór metody płatności
    Biletomat->>System Bankowy: Weryfikacja dostępnej metody
    alt metoda dostępna
    System Bankowy->>Biletomat: Poprawna metoda
    Biletomat->>Użytkownik: 
    Użytkownik->>Biletomat: Dokonaj płatności
    Biletomat->>System Bankowy: Wykonaj Płatność
    alt Brak środków
    System Bankowy->>Biletomat: Błąd transakcji
    Biletomat ->> Użytkownik: Wyświetl błąd "brak środków" 
    end
    System Bankowy->>Biletomat: Potwierdzenei transakcji
    Biletomat-->>Użytkownik:  Wyświetl potwierdzenie płatności
    Biletomat->>Użytkownik: Wydaj bilety
    else metoda nie dostępna
    System Bankowy->>Biletomat: Niedostępna metoda
    Biletomat ->> Użytkownik: Wyświetla błąd niedostępności medoty płatności
    end
```
# DIAGRAMY KLAS

## OPIS KLAS Płatność za bilet
### KLASY
#### PAYMENTMETHOD  
- **ATRYBUTY:** `STRING METHODNAME`, `BOOLEAN ISAVAILABLE` , `INT ID` 
- **METODY:**  
  - `BOOL CHECKAVAILABILITY()`  

#### PAYMENT  
- **ATRYBUTY:** `DOUBLE AMOUNT`, `STRING STATUS`, `DATE TRANSACTIONDATE`, `INT ID`  
- **METODY:**  
  - `BOOLEAN PROCESSPAYMENT()`  
  - `VOID CANCELPAYMENT()`

#### USER  
- **ATRYBUTY:** `INT ID`  
- **METODY:**  
  - `VOID INITIATEPAYMENT(PAYMENT PAYMENT)`
  - `PAYMENTMETHOD CHOOSE_METHOD()`  
  - `VOID CANCELTRANSACTION(PAYMENT PAYMENT)`
  - `VOID CLAIM_TICKETS()`

####  TICKET_MACHINE
- **ATRYBUTY:** `INT MACHINE_ID`, `BOOLEAN ISAVAILABLE` 
- **METODY:**  
  - `BOOL CHECK_PAYMENTMETHOD_AVAILABILITY(PAYMENTMETHOD METHOD)`  
  - `VOID DISPLAYCONFIRMATION(PAYMENT PAYMENT)`
  - `VOID PRINT_TICKETS`
  - `VOID SHOW_ERROR()`

#### BANK_SYSTEM  
- **ATRYBUTY:** `LIST<PAYMENTMETHOD> AVAILABLEMETHODS`  
- **METODY:**
  - `BOOL CHECK_PAYMENTMETHOD_AVAILABILITY(PAYMENTMETHOD METHOD)`
  - `VOID VERIFYMETHOD(PAYMENTMETHOD METHOD)`  
  - `BOOL CONFIRMPAYMENT(PAYMENT PAYMENT)`

### RELACJE:
- `USER` jest powiązany z `PAYMENT` (asocjacja).
- `TICKET_MACHINE` korzysta z `BANK_SYSTEM` do weryfikacji transakcji.
-  `TICKET_MACHINE` korzysta z `BANK_SYSTEM` do sprawdzenia dostępnych metod transakcji.
- `BANK_SYSTEM` zarządza `PAYMENTMETHOD` i weryfikuje dostępność metod płatności.
- `BANK_SYSTEM` zarządza `PAYMENT` i potwierdza transakcje.
- `PAYMENT` ma przypisane `PAYMENTMETHOD`



```mermaid
classDiagram

    Payment --> User: Ma przypisanego
    User --> TicketMachine: Używa
    TicketMachine -- BankSystem: komunikuje się
    Payment *-- PaymentMethod
    BankSystem -- PaymentMethod
    BankSystem --> Payment: zarządza


    class User{
      -int id
      +void InitiatePayment(Payment payment)
      +PaymentMethod ChooseMethod()
      +void CancelTransaction(Payment payment)
      +void ClaimTickets()
    }

    class PaymentMethod{
      -String methodName
      -bool isAvailable
      -int id
      +bool CheckAvailabilyty()
      
    }
    class Payment{
      -int id
      -double amount
      -String status
      -Date transactionDate

      +bool processPayment()
      +void cancelPayment()
    }
    class TicketMachine{
      -int machineId
      -bool isAvailable
      
      +bool ChackPaymentMethodAvailability()
      +void DisplayConfirmation(Payment payment)
      +void PrintTickets()
      +void ShowError()
    }

    class BankSystem{
      -List<PaymentMethod> availableMethods
      +bool CheckPaymentMethodAvailability()
      +void verifyMethod()
      +bool ConfirmPayment()
    }
```

## OPIS KLAS Zmiana języka
### KLASY
#### USER  
**METHODS:**  
- `VOID openLanguageChangeWindow()`: Opens the language change window for the user.  
- `VOID selectLanguage()`: Allows the user to select a language.  
- `VOID cancelLanguageChange()`: Cancels the language change process.  

#### USERINTERFACE  
**ATTRIBUTES:**  
- `STRING currentLanguage`: Stores the current language selected by the user.  

**METHODS:**  
- `VOID showLanguageChangeWindow()`: Displays the language change window.  
- `VOID setLanguage(STRING language)`: Sets the language in the system based on user selection.  
- `VOID closeLanguageChangeWindow()`: Closes the language change window.  
- `VOID displayAvailableLanguages(LIST<String> languages)`: Displays a list of available languages for the user to choose from.  

#### APPLICATIONSERVER  
**METHODS:**  
- `LIST<String> getLanguageList()`: Retrieves a list of available languages from the application server.  

#### DATABASE  
**METHODS:**  
- `LIST<String> getLanguageList()`: Fetches the list of available languages stored in the database.  

---

### RELATIONSHIPS:

- `USER` uses `USERINTERFACE` (ASSOCIATION).  
- `USERINTERFACE` communicates with `APPLICATIONSERVER` (ASSOCIATION).  
- `APPLICATIONSERVER` retrieves data from `DATABASE` (ASSOCIATION).


```mermaid
classDiagram

%% Classes
class User {
  + openLanguageChangeWindow(): void
  + selectLanguage(): void
  + cancelLanguageChange(): void
}

class UserInterface {
  - currentLanguage: String
  + showLanguageChangeWindow(): void
  + setLanguage(language: String): void
  + closeLanguageChangeWindow(): void
  + displayAvailableLanguages(languages: List&lt;String>): void
}

class ApplicationServer {
  + getLanguageList(): List&lt;String>
}

class Database {
  + getLanguageList(): List&lt;String>
}

%% Relationships
User --> UserInterface : uses
UserInterface --> ApplicationServer : communicates with
ApplicationServer --> Database : retrieves data

%% Attributes and Helper Methods
class Language {
  - id: int
  - name: String
}
```

