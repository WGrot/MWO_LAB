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
    D([Anulowanie transakcji])  
    F(["Wybór języka"])
    G(["Domyślny język"])
    H(["Lista popularnych języków"])
   
    F -.-> |Include|G
    H <-.- |Extend|F
    F -.-> |Include|D
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
    A[Użytkownik] -->B([Płatność za bilet])
    B -.-> |Include| C([Weryfikacja metody płatnośći])
    B -.-> |Include| D([Anulowanie transakcji])
    E([Obsługa błędów płatnośći]) -.-> |extends| B
```

### Wspólny diagram
```mermaid
flowchart TD
    D([Anulowanie transakcji])
    E([Obsługa błędów płatnośći]) -.-> |Extends| B
    A[Użytkownik] --- B([Płatność za bilet])
    B -.-> |Include| C([Weryfikacja metody płatnośći])
    B -.-> |Include| D
   

    F(["Wybór języka"])
    G(["Domyślny język"])
    H(["Lista popularnych języków"])
    A --- F
    
    F -.-> |Include|G
    F <-.- |Extend|H
    F -.-> |Include|D

 
```
