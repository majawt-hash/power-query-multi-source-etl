# 🔄 Power Query Multi-Source Data Consolidation & Automated ETL

Automatyczny proces **ETL (Extract, Transform, Load)** w **Power Query (Excel)**, który konsoliduje dane rozproszone w kilkudziesięciu plikach źródłowych, standaryzuje ich strukturę i zasilają automatyczne modele przeliczeniowe.

---

## 🎯 Problem Biznesowy
Raportowanie wymagało corocznego lub miesięcznego zbierania danych z ponad 30 osobnych arkuszy Excela od różnych zespołów/oddziałów. Pliki różniły się często formatowaniem, zawierały puste wiersze oraz zbędne kolumny, a ich ręczne scalanie zajmowało wiele godzin i tworzyło ryzyko przeoczenia rekordów.

## 💡 Rozwiązanie
Zbudowanie zapytania Power Query pobierającego dane bezpośrednio z katalogu sieciowego:
1. **Automatyczne scalanie plików (Folder Connector):** Łączenie 30+ plików Excel w jeden ciągły strumień danych bez konieczności ich ręcznego otwierania.
2. **Transformacja i Czyszczenie (M Code):**
   * Usuwanie nagłówków, pustych wierszy i niepotrzebnych metadanych.
   * Unifikacja typów danych (daty, liczby, waluty) oraz usuwanie białych znaków.
   * Filtrowanie i sortowanie według reguł biznesowych.
3. **Automatyczny Calculation Engine:** Wstrzyknięcie czystych danych do modelu Excela, gdzie dedykowane formuły automatycznie kalkulują kluczowe wskaźniki (KPI) i zagregowane sumy.

---

## 🛠️ Stos Technologiczny
* **Narzędzie:** Microsoft Power Query (Excel)
* **Język transformacji:** M Code
* **Model danych:** Excel Formulas & Automated Tables

---

## 🏗️ Architektura Przepływu Danych

```text
[ Folder: 30+ Source Excel Files ]
               │
               ▼
   [ Power Query Folder Connector ]
               │
               ▼
   [ Data Cleansing & Unification (M Code) ]
   ├── Remove top/empty rows
   ├── Standardize column data types
   └── Filter out noise
               │
               ▼
   [ Unified Output Table in Excel ]
               │
               ▼
   [ Automated Formulas & KPI Engine ]
