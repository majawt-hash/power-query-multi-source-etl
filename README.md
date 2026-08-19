# 🔄 Power Query Billing Data ETL & PPE Meter Consolidation

Automatyczny proces **ETL (Extract, Transform, Load)** w **Power Query (Excel)**, który co miesiąc konsoliduje dzienne pliki rozliczeniowe, standaryzuje uszkodzone formaty danych (numery PPE, znaki specjalne przy zużyciu) i automatycznie wylicza bilans energii dla każdego punktu poboru (PPE).

---

## 🎯 Problem Biznesowy
Na skrzynkę e-mail codziennie spływają dzienne pliki rozliczeniowe w formacie Excel. Co miesiąc wymagało to ręcznego scalania kilkudziesięciu arkuszy i przygotowywania podsumowań dla każdego punktu poboru energii (PPE) osobno. 

Dodatkowo dane źródłowe zawierały krytyczne błędy formatowania:
* **Zniekształcone numery PPE:** Długie identyfikatory były automatycznie konwertowane przez Excela na notację naukową i zaokrąglane (np. `5,90123E+17`).
* **Niestandardowy zapis zużycia:** Wartości zużycia zawierały znaki specjalne i sufiksy (np. `0.05,+`), co uniemożliwiało wykonywanie jakichkolwiek obliczeń matematycznych.

## 💡 Rozwiązanie
Zbudowanie automatycznego potoku przetwarzania w Power Query wraz z modelem zagregowanym w Excelu:

1. **Konsolidacja wsadowa (Folder Connector):** Co miesiąc wszystkie dzienne pliki Excel są wrzucane do jednego folderu, a Power Query scala je w jeden ciągły strumień danych za jednym kliknięciem.
2. **Transformacja i Czyszczenie (M Code):**
   * **Naprawa numerów PPE:** Konwersja i wymuszenie typu tekstowego przed przetwarzaniem, zapobiegające zaokrąglaniu ciągów cyfr.
   * **Standaryzacja danych o zużyciu:** Czyszczenie ciągów znaków (parsing znaków `,+`), usunięcie błędów i konwersja na czyste wartości liczbowe (Decimal Number) gotowe do obliczeń.
   * **Chronologia:** Filtrowanie szumów oraz sortowanie danych według daty.
3. **Automatyczny Model Bilansujący (Excel Template):**
   Dedykowana zakładka szablonu automatycznie:
   * Wyciąga listę unikalnych numerów PPE.
   * Agreguje zużycie dla każdego PPE z osobna.
   * Wylicza sumę całkowitą, sumę wartości dodatnich (pobór) oraz ujemnych (generacja / produkcja > zużycie).

---

## 🛠️ Stos Technologiczny
* **Narzędzie:** Microsoft Power Query (Excel)
* **Język transformacji:** M Code
* **Model danych:** Excel Dynamic Formulas (UNIQUE, SUMIFS) / Automated Pivot Engine

---

## 🏗️ Architektura Przepływu Danych

```text
[ Monthly Folder with Daily Excel Files ]
                    │
                    ▼
     [ Power Query Folder Connector ]
                    │
                    ▼
    [ Cleansing & Transformation (M Code) ]
    ├── Text Formatting: Fix PPE Large Numbers
    ├── String Parsing: Clean '0.05,+' -> Decimal
    └── Sort & Filter by Date
                    │
                    ▼
     [ Unified Output Table (Excel) ]
                    │
                    ▼
[ Billing Template (Unique PPEs & Sums) ]
    ├── Total Consumption per PPE
    ├── Positive Sum (Grid Draw)
    └── Negative Sum (Production Output)
