# Dokument wymagań produktu (PRD) - HACCP Manager (MVP)
## 1. Przegląd produktu
HACCP Manager to webowa aplikacja dla małej gastronomii w Polsce (bary, kawiarnie, food-trucki), która umożliwia cyfrowe prowadzenie i archiwizację rejestrów GHP/GMP oraz uproszczonego planu HACCP. MVP skupia się na szybkim, zgodnym z wymogami Sanepidu wprowadzaniu danych (temperatury, sprzątanie, szkodniki, przeglądy), elastycznych checklistach, raportowaniu na kontrolę oraz prostych powiadomieniach.

Założenia techniczne (informacyjne): backend Django REST + Postgres, (Celery + Redis w przyszłości) synchroniczne genergowanie raportów do zadań, frontend Vue 3 + Quasar, kontenery Docker. Aplikacja przystosowana do desktopu i telefonu (RWD). Język interfejsu: PL.

Role i uprawnienia:
- pracownik: pełny dostęp do konfiguracji, raportów i historii, może edytować dane historyczne; jedyny, który może edytować progi krytyczne globalnie i per urządzenie; jedyny


Roadmapa MVP (ok. 10 h/tyg., brak budżetu startowego):
- Iteracja I: uwierzytelnianie i autoryzacja, rejestr temperatur, PDF „Pobierz Raport”.
- Iteracja II: rejestry sprzątania i szkodników, konfiguracja progów per typ z dziedziczeniem, powiadomienia e-mail/in-app, widok zaległości.
- Iteracja III: rejestr przeglądów sprzętu, edytowalne checklisty GHP/GMP, audyt zmian.

## 2. Problem użytkownika
Małe punkty gastronomiczne muszą prowadzić i przechowywać liczne rejestry (co najmniej 5 lat), co w praktyce odbywa się w papierowych zeszytach. Skutkuje to:
- czasochłonnym i podatnym na błędy procesem wpisów,
- brakiem walidacji pól wymaganych przepisami,
- trudnością w szybkim przygotowaniu kompletnych dokumentów na kontrolę Sanepidu,
- brakiem spójnych progów krytycznych i wymuszania działań korygujących,
- brakiem powiadomień o zaległych czynnościach i przeglądach.

HACCP Manager rozwiązuje te problemy poprzez: proste ścieżki szybkiego wpisu, walidację i wymuszenie wymaganych pól, konfigurację progów, generowanie raportów z presetami 30/90 dni, przypomnienia oraz bezpieczną archiwizację.

## 3. Wymagania funkcjonalne
3.1 Uwierzytelnianie i autoryzacja
- Rejestracja, logowanie, reset hasła.
- Pracownik moze edytować/ustawiać progi krytyczne i zmieniać konfigurację urządzeń oraz usuwać.
- Sesje i polityka wylogowania po bezczynności.

3.2 Konfiguracja lokalu i plan HACCP (uproszczony)
- Profil lokalu: nazwa, NIP opcjonalnie, adres, opis pomieszczeń i urządzeń.
- Urządzenia chłodnicze/grzewcze: typy (np. chłodnia, zamrażarka, piec, lada chłodnicza), lokalizacja/pomieszczenie, identyfikator.
- Progi krytyczne: globalne per typ urządzenia (dziedziczenie), możliwość nadpisania per konkretne urządzenie; edytowalne.
- Statyczne poradniki: 7 zasad HACCP i 12 etapów wdrażania (podsumowanie w aplikacji).

3.3 Rejestry GHP/GMP
- Rejestr temperatur:
  - Wpis dzienny lub per zmiana (konfigurowalna częstotliwość minimalna, domyślnie co najmniej raz/dobę).
  - Walidacja pól: data, godzina, urządzenie, wartość, osoba wprowadzająca.
  - Alert i wymuszenie działań korygujących przy przekroczeniu progu (opis działania, czas, osoba).
  - Historia zmian (kto, kiedy, co zmienił).
- Rejestr sprzątania i dezynfekcji:
  - Terminarz zadań (mycie, dezynfekcja), częstotliwość, odpowiedzialny.
  - Wpis statusu: wykonano/nie wykonano/nie dotyczy, notatka, data/godzina.
- Rejestr szkodników:
  - Inspekcje (data, obserwacje), zastosowane działania, dostawca usług (opcjonalnie).
  - Status i notatki.
- Rejestr przeglądów sprzętu:
  - Zapisy serwisów/napraw/kalibracji: data, wykonawca, wynik, załączona notatka.
  - Przypomnienia o kalibracji/przeglądzie (harmonogram).

3.4 Listy kontrolne GHP/GMP
- Zestaw gotowych checklist (higiena osobista, czyszczenie i dezynfekcja, woda, odpady, monitoring szkodników, szkolenia).
- Dostosowanie list przez pracownika: dodawanie/usuwanie pozycji, zmiana częstotliwości. W pełni edytowalne checklisty wchodzą w iteracji III (częściowy widok w MVP jako gotowe listy).

3.5 Powiadomienia i harmonogramy
- Harmonogramy per lista/urządzenie (np. raz/doba, co zmianę, dni robocze).
- Powiadomienia in-app i e-mail (włącz/wyłącz per użytkownik).
- Widok zaległości: zadania/wpisy po terminie, filtrowanie po typie rejestru/urządzeniu/osobie.

3.6 Raportowanie i archiwizacja
- „Pobierz Raport”: presety zakresów 30/90 dni, generowanie jednego PDF z kluczowymi rejestrami i załączonym podsumowaniem działań korygujących; możliwość eksportu ZIP (PDF + CSV-y per rejestr).
- Raporty per urządzenie/pomieszczenie/typ rejestru (PDF/CSV).
- Archiwizacja wszystkich danych przez min. 5 lat.
- Eksport danych na żądanie pracownika (CSV, ZIP).

3.7 Logika biznesowa i walidacja
- Wymuszenie pól wymaganych przepisami: data, godzina, temperatura/parametr, urządzenie, osoba, podpis elektroniczny (imię i nazwisko użytkownika oraz znacznik czasu).
- Działania korygujące wymagane przy przekroczeniu progów (nie można zamknąć wpisu bez uzupełnienia).
- Nienaruszalność danych: audyt zmian (kto, co, kiedy), pracownik może poprawiać historyczne wpisy z pełnym śladem audytowym.

3.8 Bezpieczeństwo i RODO
- Minimalizacja danych osobowych (tylko dane pracownicze konieczne do rejestrów).
- Szyfrowanie danych w spoczynku i w tranzycie (TLS), polityki dostępu per rola.
- Dzienniki audytowe i możliwość wygenerowania rejestru czynności przetwarzania (na poziomie organizacyjnym).
- Kopie bezpieczeństwa bazy danych i test odtworzeniowy.

## 4. Granice produktu
W zakresie MVP:
- Uwierzytelnianie, role i uprawnienia.
- Konfiguracja lokalu i urządzeń z progami (globalne per typ i nadpisy per urządzenie).
- Rejestry: temperatury (priorytet), sprzątanie, szkodniki, przeglądy.
- Powiadomienia e-mail oraz in-app (prostą logiką).
- Raporty „Pobierz Raport” (30/90 dni, PDF + opcjonalny ZIP), raporty per urządzenie/typ.
- Archiwizacja 5 lat, eksport CSV.
- Audyt zmian i blokady edycji zgodnie z rolą.

Poza zakresem MVP:
- Zamknięcie miesiąca i workflow rozliczeniowy.
- Integracje z IoT (sensory), POS, księgowość, magazyn.
- Wielolokalowość i zaawansowana analityka multi-site.
- Składanie wniosków o certyfikaty, audyty zewnętrzne.
- E-learning i przechowywanie certyfikatów/książeczek.
- Repozytorium dokumentów zewnętrznych (umowy, wyniki badań wody itd.).
- Aplikacja mobilna natywna (RWD wystarczy).
- Formalny szablon raportu Sanepidu z wzorcami urzędowymi – do doprecyzowania w późniejszej iteracji (zostają raporty zgodne merytorycznie).

Ryzyka i kwestie otwarte:
- Brak ujednoliconego formalnego wzorca raportu Sanepidu (układ tabel, pieczęcie). W MVP: komplet wymaganych pól + klarowny layout, a doprecyzowanie formatów urzędowych planowane po zebraniu wzorców.

## 5. Historyjki użytkowników
US-001  
Tytuł: Rejestracja i logowanie  
Opis: Jako użytkownik chcę założyć konto i zalogować się do systemu, aby korzystać z aplikacji zgodnie z moją rolą.  
Kryteria akceptacji:
- Użytkownik może utworzyć konto z e-mailem i hasłem.
- Użytkownik może zalogować się i wylogować.
- Reset hasła działa poprzez e-mail.
- Po zalogowaniu system prezentuje funkcje zgodne z rolą.


US-003  
Tytuł: Konfiguracja lokalu i urządzeń  
Opis: Jako pracownik chcę dodać pomieszczenia i urządzenia z typami, aby móc prowadzić rejestry i raporty per urządzenie.  
Kryteria akceptacji:
- Dodawanie/edycja/usuwanie (soft-delete) urządzeń: nazwa, typ, lokalizacja.
- Lista urządzeń jest filtrowalna po pomieszczeniu/typie.
- Urządzenie może być oznaczone jako aktywne/nieaktywne.

US-004  
Tytuł: Progi krytyczne per typ z dziedziczeniem  
Opis: Jako pracownik chcę ustawić progi krytyczne globalnie dla typu urządzenia oraz nadpisać je per urządzenie.  
Kryteria akceptacji:
- Ustawienie progu min/max na typie urządzenia.
- Urządzenie domyślnie dziedziczy wartości z typu.
- Nadpisanie na urządzeniu jest widoczne w formularzach i raportach.
- Tylko pracownik może edytować progi.

US-005  
Tytuł: Szybki wpis temperatury  
Opis: Jako pracownik chcę szybko przejść przez listę urządzeń i wpisać temperatury, aby zrealizować obowiązek dzienny.  
Kryteria akceptacji:
- Widok listy urządzeń aktywnych z szybkim formularzem (data/godzina, wartość).
- Walidacja pól wymaganych (data, godzina, urządzenie, wartość).
- Zapis oznacza użytkownika i znacznik czasu.
- Pracownik edytuje tylko wpisy z bieżącego dnia.

US-006  
Tytuł: Alert i działanie korygujące  
Opis: Jako pracownik chcę, aby system wymagał działań korygujących przy przekroczeniu progów, aby spełnić zasady HACCP.  
Kryteria akceptacji:
- Wpis powyżej/progi powoduje czerwony alert.
- Zanim zapisze się wpis, wymagane jest pole „Działanie korygujące”.
- W raporcie widoczne są działania korygujące (kto, co, kiedy).

US-007  
Tytuł: Historia zmian i audyt  
Opis: Jako pracownik chcę widzieć historię zmian wpisów, aby wykazać nienaruszalność danych.  
Kryteria akceptacji:
- Każda zmiana zapisuje: kto, kiedy, stare i nowe wartości.
- Podgląd historii dostępny per wpis/urządzenie.

US-008  
Tytuł: Rejestr sprzątania  
Opis: Jako pracownik chcę odnotować wykonanie zadań czystości z terminarza.  
Kryteria akceptacji:
- Lista zadań z częstotliwościami (np. codziennie, co zmianę).
- Wpis statusu: wykonano/nie wykonano/nie dotyczy, notatka, data/godzina.
- Brak możliwości zapisu bez daty/godziny i osoby.
- Widok zaległości pokazuje niewykonane zadania.

US-009  
Tytuł: Rejestr szkodników  
Opis: Jako pracownik chcę wpisać obserwacje i działania dotyczące szkodników.  
Kryteria akceptacji:
- Formularz inspekcji: data, obserwacje, działania.
- Status i notatka są widoczne w raportach.
- Walidacja pól wymaganych i zapis osoby.

US-010  
Tytuł: Rejestr przeglądów sprzętu  
Opis: Jako pracownik chcę dokumentować przeglądy, naprawy i kalibracje, aby być gotowym na kontrolę.  
Kryteria akceptacji:
- Wpis: data, rodzaj przeglądu/kalibracji, wykonawca, wynik, notatka.
- Możliwość ustawienia przypomnienia na kolejną datę.
- Widok zaległości uwzględnia terminy przeglądów.

US-011  
Tytuł: „Pobierz Raport” – raport 30/90 dni  
Opis: Jako pracownik chcę jednym kliknięciem wygenerować PDF z ostatnich 30/90 dni.  
Kryteria akceptacji:
- Presety 30 i 90 dni oraz zakres własny.
- Jeden PDF zawiera: temperatury (z oznaczeniami przekroczeń i działaniami korygującymi), sprzątanie, szkodniki, przeglądy.
- Opcja eksportu ZIP: PDF + CSV per rejestr.
- Raport zawiera podpisy elektroniczne (kto uzupełnił rubrykę i kiedy).

US-012  
Tytuł: Raport per urządzenie/pomieszczenie  
Opis: Jako pracownik chcę wygenerować raport dla wybranego urządzenia lub pomieszczenia.  
Kryteria akceptacji:
- Filtry: urządzenie, pomieszczenie, typ rejestru, zakres dat.
- Eksport PDF/CSV.
- Widoczne progi oraz oznaczenia przekroczeń.

US-013  
Tytuł: Harmonogramy i powiadomienia  
Opis: Jako pracownik chcę skonfigurować harmonogramy i otrzymywać powiadomienia o zaległościach.  
Kryteria akceptacji:
- Harmonogramy: raz/doba, co zmianę, w dni robocze; przypięte do list/urządzeń.
- Powiadomienia in-app i e-mail (można włączyć/wyłączyć).
- Widok zaległości agreguje zadania po terminie.

US-015  
Tytuł: Walidacja wymaganych pól  
Opis: Jako użytkownik chcę, by system nie pozwolił zapisać formularza bez wymaganych pól.  
Kryteria akceptacji:
- Brak daty/godziny/wartości/urządzenia/osoby blokuje zapis.
- Czytelne komunikaty o brakach.
- Focus na pierwsze brakujące pole.

US-016  
Tytuł: Dostęp na urządzeniach mobilnych (RWD)  
Opis: Jako pracownik chcę wygodnie korzystać z aplikacji na telefonie.  
Kryteria akceptacji:
- Widoki „Szybki wpis temperatury” i „Zadania na dziś” działają płynnie na małych ekranach.
- Formularze mieszczą się na ekranie bez poziomowego przewijania.

US-017  
Tytuł: Eksport i archiwizacja 5 lat  
Opis: Jako pracownik chcę mieć gwarancję archiwizacji wpisów oraz możliwość eksportu danych.  
Kryteria akceptacji:
- System przechowuje wpisy co najmniej 5 lat.
- Eksport CSV i ZIP dostępny z zakresami dat.
- Informacja o powodzeniu eksportu (in-app/e-mail).


US-019  
Tytuł: Edycja list kontrolnych GHP/GMP (MVP zakres podstawowy)  
Opis: Jako pracownik chcę używać gotowych checklist i dostosować częstotliwości, aby odpowiadały specyfice lokalu.  
Kryteria akceptacji:
- Gotowe checklisty dostępne w systemie.
- Możliwość zmiany częstotliwości na poziomie pozycji listy.
- Pełna edycja treści pozycji i struktury list w iteracji III.

US-020  
Tytuł: Widok zaległości  
Opis: Jako pracownik chcę widzieć opóźnione wpisy i zadania, by szybko je domknąć.  
Kryteria akceptacji:
- Widok agreguje zaległe temperatury, sprzątanie, szkodniki, przeglądy.
- Filtrowanie po typie rejestru, urządzeniu, osobie, pomieszczeniu.
- Możliwość wysłania przypomnienia użytkownikowi.

US-021  
Tytuł: Zmiana minimalnej częstotliwości wpisów temperatur  
Opis: Jako pracownik chcę ustawić minimalną wymaganą częstotliwość wpisów per urządzenie lub typ (np. co zmianę), aby dopasować się do wymogów.  
Kryteria akceptacji:
- Konfiguracja częstotliwości na typie urządzenia i nadpisanie per urządzenie.
- Widok zaległości uwzględnia tę częstotliwość.
- Walidacja przy próbie zamknięcia dnia bez wymaganej liczby wpisów.


US-024  
Tytuł: Filtrowanie i wyszukiwanie wpisów  
Opis: Jako pracownik chcę szybko znaleźć wpisy po dacie, typie rejestru, urządzeniu
Kryteria akceptacji:
- Filtry wielokrotne i wyszukiwanie tekstowe po notatkach.
- Eksport wyników filtrowania do CSV.


US-026  
Tytuł: Widok „Zadania na dziś”  
Opis: Jako pracownik chcę widzieć listę dzisiejszych obowiązków (temperatury, sprzątanie itd.), aby wykonać je na czas.  
Kryteria akceptacji:
- Zbiorcza lista z sumą zadań i stanem realizacji.
- Linki do szybkich formularzy wpisu.

US-027  
Tytuł: Personalizacja powiadomień e-mail  
Opis: Jako użytkownik chcę włączyć/wyłączyć e-mail o zaległościach, aby nie być spamowanym.  
Kryteria akceptacji:
- Przełącznik powiadomień w profilu użytkownika.
- Zmiana ma natychmiastowy efekt na kolejne wysyłki.

US-028  
Tytuł: Podsumowanie 7 zasad HACCP i 12 etapów  
Opis: Jako użytkownik chcę mieć szybki podgląd zasad i etapów wdrażania.  
Kryteria akceptacji:
- Strona z krótkim, statycznym poradnikiem.
- Łatwa nawigacja do powiązanych formularzy.

US-029  
Tytuł: Spójny podpis elektroniczny wpisu  
Opis: Jako system chcę dodawać do każdego wpisu podpis elektroniczny użytkownika (imię i nazwisko) oraz znacznik czasu.  
Kryteria akceptacji:
- Podpis i timestamp są widoczne w szczegółach wpisu i raportach.
- Dane są nieedytowalne dla pracownika po zapisie (poza bieżącym dniem).

US-030  
Tytuł: Raport błędów walidacji  
Opis: Jako użytkownik chcę zobaczyć czytelne komunikaty walidacyjne, aby poprawić formularz.  
Kryteria akceptacji:
- Komunikaty wskazują konkretne brakujące pola.
- Focus na pierwszym błędnym polu po wysłaniu formularza.

US-031  
Tytuł: Soft-delete i przywracanie  
Opis: Jako pracownik chcę móc oznaczyć urządzenie lub zadanie jako nieaktywne bez utraty historii.  
Kryteria akceptacji:
- Soft-delete ukrywa element w widokach operacyjnych.
- Historia pozostaje dostępna i raportowalna.
- Możliwość przywrócenia elementu.


US-034  
Tytuł: Wyszczególnienie częstotliwości wpisów w raportach  
Opis: Jako pracownik chcę widzieć w raporcie wymaganą częstotliwość wpisów i faktyczne wykonanie.  
Kryteria akceptacji:
- Raport pokazuje wymaganą vs. wykonaną liczbę wpisów per okres i urządzenie.
- Braki są oznaczone wizualnie.


US-037  
Tytuł: Podgląd dzienny „Szybki wpis temperatury”  
Opis: Jako pracownik chcę podejrzeć, które urządzenia mają już wpis z dziś.  
Kryteria akceptacji:
- Lista urządzeń z oznaczeniem stanu (wpis wykonany/oczekuje).
- Przejście do edycji wpisu z dziś (jeśli rola pozwala).

US-038  
Tytuł: Notatki do wpisów  
Opis: Jako użytkownik chcę dodać krótką notatkę do każdego wpisu.  
Kryteria akceptacji:
- Pole notatki jest opcjonalne, ale limitowane znakami.
- Notatka jest widoczna w raporcie szczegółowym.

US-039  
Tytuł: Wyświetlanie progów w formularzu  
Opis: Jako pracownik chcę widzieć w formularzu aktualny próg dla urządzenia.  
Kryteria akceptacji:
- Formularz pokazuje min/max aktualnie obowiązujące (z dziedziczenia lub nadpisania).
- W przypadku nadpisania wyświetlany jest znacznik „lokalny próg”.

US-040  
Tytuł: Rejestr i raport szkodników z działaniami  
Opis: Jako pracownik chcę mieć listę działań podjętych w reakcji na obserwacje.  
Kryteria akceptacji:
- Raport zestawia obserwacje i działania z datami.
- Możliwość filtrowania po rodzaju szkodnika i obszarze.

US-042  
Tytuł: Włączenie/wyłączenie powiadomień  
Opis: Jako użytkownik chcę globalnie włączyć/wyłączyć powiadomienia e-mail.  
Kryteria akceptacji:
- Przełącznik w profilu.
- Zmiana odzwierciedlona w kolejnych wysyłkach.

US-045  
Tytuł: Podgląd konfiguracji progów w raporcie  
Opis: Jako kontrolujący chcę widzieć, jakie progi obowiązywały w danym okresie.  
Kryteria akceptacji:
- Raport zawiera sekcję „Progi obowiązujące w okresie” z ustawionym progiem

US-046  
Tytuł: Filtrowanie działań korygujących  
Opis: Jako pracownik chcę przeglądać i filtrować działania korygujące po typie urządzenia, użytkowniku i dacie.  
Kryteria akceptacji:
- Widok listy działań z filtrami.
- Eksport CSV.

US-047  
Tytuł: Blokada zamknięcia wpisu bez działań korygujących  
Opis: Jako system chcę zablokować zapis wpisu temperatury, gdy jest przekroczenie bez opisu korekty.  
Kryteria akceptacji:
- Walidacja twarda przed zapisem.
- Komunikat informuje, które pole należy wypełnić.

US-048  
Tytuł: Jednolity layout raportów  
Opis: Jako użytkownik chcę spójny layout raportów, aby były czytelne dla Sanepidu.  
Kryteria akceptacji:
- Raporty mają nagłówek z danymi lokalu, zakres dat, legendę statusów i sekcję podpisów.
- Czcionki i tabela są czytelne w druku A4.

US-049  
Tytuł: Rejestrowanie wykonywania harmonogramu  
Opis: Jako system chcę odnotować wysyłkę przypomnienia i jego status.  
Kryteria akceptacji:
- Każde przypomnienie ma timestamp wysyłki i status (np. dostarczono/błąd).
- Widoczne w logu zadań systemowych.

US-051  
Tytuł: Raport kompletności dziennej  
Opis: Jako pracownik chcę widzieć, czy wszystkie wymagane dziś wpisy zostały wykonane.  
Kryteria akceptacji:
- Widok procentowej realizacji per rejestr/urządzenie.
- Lista braków z linkami do formularzy.

US-052  
Tytuł: Rejestrowanie niepowodzeń e-mail  
Opis: Jako system chcę logować błędy wysyłki maila.  
Kryteria akceptacji:
- Błąd wysyłki jest logowany z przyczyną (o ile znana).
- pracownik widzi listę ostatnich błędów.

US-053  
Tytuł: Odtwarzalność raportu w czasie  
Opis: Jako pracownik chcę mieć pewność, że raport wygenerowany później odzwierciedla ówczesne progi i dane.  
Kryteria akceptacji:
- System zapisuje wersje progów w czasie (stempel wersji).
- Raport za przeszły okres używa właściwych progów historycznych.

US-055  
Tytuł: Wgląd pracownika we własne wpisy  
Opis: Jako pracownik chcę przeglądać swoje dzisiejsze wpisy.  
Kryteria akceptacji:
- Lista wpisów z filtrem „moje”.
- Możliwość edycji tylko do końca dnia.

US-056  
Tytuł: Wsparcie dla statusu „nie dotyczy”  
Opis: Jako użytkownik chcę oznaczyć pozycję jako N/D, gdy nie ma zastosowania.  
Kryteria akceptacji:
- Status N/D dostępny w rejestrach i checklistach.
- N/D pojawia się w raportach z legendą.


US-060  
Tytuł: Dostęp do statycznego poradnika HACCP/GHP/GMP  
Opis: Jako nowy pracownik chcę szybko zrozumieć kontekst rejestrów.  
Kryteria akceptacji:
- Poradnik dostępny z poziomu głównego menu.
- Sekcje z linkami do odpowiednich rejestrów.

## 6. Metryki sukcesu
Lista kontrolna PRD:
- Wszystkie historyjki są testowalne i mają jednoznaczne kryteria akceptacji.
- Ujęto scenariusze podstawowe, alternatywne i skrajne (np. brak łącza, brak uprawnień).
- Ujęto wymagania dot. uwierzytelniania i autoryzacji (US-001, US-002, US-033, US-036).
- Zakres MVP i granice są jasno określone; istnieje roadmapa iteracji.
