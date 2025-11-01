# Aplikacja – HACCP Manager (MVP)

## Główny problem

Małe lokale gastronomiczne w Polsce (bary, kawiarnie, food‑trucki) muszą prowadzić dokumentację z zakresu GHP/GMP i HACCP. Obowiązek ten wynika z rozporządzenia (WE) 852/2004 i krajowych przepisów sanitarnych – przedsiębiorca musi identyfikować zagrożenia, monitorować procesy, prowadzić rejestry higieny i przechowywać je przez minimum 5 lat. Tradycyjne „zeszyty HACCP” są uciążliwe i narażone na błędy. Celem projektu jest uproszczenie tych czynności poprzez narzędzie umożliwiające cyfrowe prowadzenie rejestrów i planu HACCP, dostosowane do potrzeb małej gastronomii.

## Najmniejszy zestaw funkcjonalności

1. Mechanizm kontroli dostępu*
   * Ekran logowania z możliwością tworzenia konta.  

2. Konfiguracja i uproszczony plan HACCP
   * Formularz wprowadzający podstawowe dane o lokalu (nazwa, adres, opis pomieszczeń, urządzeń).  
   * Wbudowane listy kontrolne oparte na 12 etapach wdrażania HACCP oraz ogólnych wytycznych GHP/GMP. Użytkownik może zaznaczać status („wykonano”, „niewykonano”, „nie dotyczy”) i dodać opis lub działanie korygujące.  
   * Możliwość edycji list (dodawanie własnych elementów lub ukrywanie zbędnych), aby odzwierciedlić specyfikę lokalu.  
   * Podsumowanie 7 zasad HACCP i 12 etapów wdrażania w formie statycznego poradnika.

3. Elektroniczne rejestry GHP/GMP
   * Rejestr temperatury: możliwość wprowadzania i przechowywania pomiarów temperatur w chłodniach i urządzeniach grzewczych – co najmniej raz na dobę(mozliwa edycja wymogow ile razy na dobe musi być uzupełnione), zgodnie z wymaganiami księgi HACCP.  
   * Rejestry sprzątania i dezynfekcji: terminarz zadań czystości (mycie, dezynfekcja) oraz możliwość odnotowania ich wykonania.  
   * Rejestr szkodników: monitorowanie obecności gryzoni i owadów oraz odnotowanie zastosowanych działań.  
   * Rejestr przeglądów sprzętu: zapisy dotyczące serwisu i kalibracji urządzeń (przeglądy, naprawy).  
   * Wszystkie rejestry mają możliwość oznaczenia statusu (wykonano/nie wykonano/nie dotyczy) i dodania opisu.

4. Listy kontrolne GHP/GMP
   * Zestaw gotowych checklist opracowanych na podstawie wymogów sanepidu: higiena osobista, czyszczenie i dezynfekcja, zaopatrzenie w wodę, kontrola odpadów, monitoring szkodników, szkolenie personelu.  
   * Możliwość dostosowania list do konkretnego lokalu (dodawanie/usuwanie pozycji, zmiana częstotliwości). 

5. Obsługa powiadomień
   * mailowe z opcją włączenie/wyłączenia

5. Raportowanie i archiwizacja  
   * Generowanie raportów PDF/CSV zawierających wprowadzone dane (np. miesięczne rejestry temperatury, checklisty higieny, lista zadań).  
   * Przechowywanie danych przez wymagane 5 lat w bezpiecznej bazie danych.

6. Logika biznesowa i walidacja  
   * Aplikacja wymusza uzupełnienie pól wymaganych przepisami (np. data, godzina, temperatura) i nie pozwala zatwierdzić formularza bez ich wypełnienia.  
   * Jeżeli temperatura lub inny parametr przekroczy ustalony limit krytyczny, system powinien poprosić o podanie działań korygujących zgodnie z zasadami HACCP.


## Co nie wchodzi w zakres MVP
- podział ról uzytkowników: pracownik, manager. Manager moze edytować wszystko, a pracownik tylko swoje wpisy z dzisiaj.
- "zamknięcie miesiąca" - tylko dla managera
- Skrócony przewodnik dla użytkowników, wyjaśniający sens 7 zasad HACCP, 12 etapów wdrażania oraz zakres GHP/GMP.
- Integracje z inteligentnymi sensorami (IoT) do automatycznego pomiaru temperatury.   
- Integracje z systemami POS, księgowymi lub magazynowymi.  
- Obsługa wielu oddziałów/franczyz i zaawansowana analityka wielolokacyjna.  
- Składanie wniosków o certyfikaty HACCP lub audyty zewnętrzne.  
- Zaawansowane szkolenia e‑learningowe – w MVP ograniczamy się do statycznych opisów i list kontrolnych.
- Rejestrowania i przechowywania certyfikatów szkoleń oraz badań pracowników – w przyszłości aplikacja może gromadzić skany zaświadczeń HACCP/BHP, książeczek sanitarno‑epidemiologicznych oraz daty ważności, wysyłając przypomnienia o konieczności odnowienia badań.
Przechowywania dokumentów zewnętrznych – np. wyników badań wody, umów i raportów z deratyzacji/dezynsekcji, certyfikatów kalibracji wag i termometrów, powiadomienia o potrzebie odnowienia dokumentów/kalibracji.
- aplikacja mobilna

## Kryteria sukcesu

1. Zgodność z przepisami – aplikacja musi umożliwiać prowadzenie rejestrów i dokumentacji wymaganych przez Sanepid, zgodnie z 7 zasadami HACCP i GHP/GMP.  
2. Użyteczność – user potrafi w ciągu kilku minut dodać wpis, oznaczyć status i wygenerować raport.  
3. Elastyczność – użytkownicy mogą modyfikować listy kontrolne i plan HACCP, aby dostosować je do specyfiki swojej gastronomii, korzystając z gotowych propozycji.  
4. Gotowość na kontrolę – podczas wizyty sanepidu user może w prosty sposób wydrukować lub udostępnić elektroniczne rejestry z ostatnich miesięcy oraz plan HACCP.  
5. Niski próg wejścia – prosta instalacja i uruchomienie (np. poprzez kontener Docker), bez konieczności długiego szkolenia użytkowników.
