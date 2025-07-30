# 🧠 Automatyzacja analizy wyciągów bankowych – Make.com + AirTable + CRM

## 📋 Opis projektu

Celem projektu jest pełna automatyzacja procesu analizy wyciągów bankowych w formacie CSV, z dalszą integracją danych w systemie CRM oraz AirTable. Workflow został zbudowany w Make.com i oparty jest na analizie każdego przelewu przychodzącego oraz wychodzącego.

Dla każdej transakcji:
- Tworzony jest odpowiedni rekord w AirTable.
- Wydatki są kategoryzowane przy użyciu OpenAI API.
- Przychody są analizowane pod kątem identyfikacji płatności za zamówienia.
- Na podstawie danych z przelewu, status zamówienia jest aktualizowany w systemie CRM.

---

## 🗂️ Struktura workflow

Workflow składa się z 6 głównych faz:

1. **Webhook – Pobranie i obróbka pliku CSV**
2. **Segregacja przelewów na wpływy i wydatki**
3. **Sprawdzenie istnienia transakcji w AirTable**
4. **Tworzenie rekordu w AirTable i/lub przypisywanie kategorii**
5. **Komunikacja z CRM**
6. **Aktualizacja statusu zamówienia**

---

## 🔄 Faza 1: Webhook – Pobranie i obróbka pliku CSV

- **Trigger**: Webhook Make.com, który odbiera plik CSV (upload przez formularz w AirTable).
- **Przetwarzanie danych**: CSV jest parsowany – każdy wiersz reprezentuje jedną transakcję.
- **Transformacja danych**: dane są czyszczone: usuwane są niepotrzebne 4 pierwsze wiersze oraz ostatni wiersz każdego pliku, tworzony jest obiekt CSV, liczby parsowane jako wartości numeryczne, zamiana przecinków na kropki.

<img width="1192" height="97" alt="image" src="https://github.com/user-attachments/assets/0ad53d75-6738-4df0-a5bd-ac577beb3283" />


---

## 🔍 Faza 2: Segregacja przelewów – wpływy i wydatki

- Transakcje są dzielone na:
  - **Wpływy (przychody)** – dodatnie wartości kwot.
  - **Wydatki (koszty)** – wartości ujemne.
- Decyzja oparta na znaku kwoty lub rodzaju transakcji.

<img width="508" height="457" alt="image" src="https://github.com/user-attachments/assets/c8128487-2f23-4c93-8485-1b6e704c56af" />
<img width="355" height="407" alt="image" src="https://github.com/user-attachments/assets/51731a04-24c3-4b23-99b5-11cf15b61422" />


---

## 🔁 Faza 3: Sprawdzenie istnienia transakcji w AirTable

- Każda transakcja sprawdzana jest pod kątem duplikatów w bazie AirTable.
- Identyfikacja po:
  - Dacie,
  - Nadawcy,
  - Tytule,
  - Kwocie.

<img width="338" height="244" alt="image" src="https://github.com/user-attachments/assets/26814565-c3d5-4057-a01f-e59c5f3dadd4" /><img width="357" height="323" alt="image" src="https://github.com/user-attachments/assets/491c11f7-20fe-4bf7-a9f9-6d933c905cce" />



---

## 🧾 Faza 4: Przetwarzanie transakcji

### 💸 Dla wydatków:

- Dane transakcji są przekazywane do OpenAI API (GPT-4) w celu przypisania odpowiedniej **kategorii wydatku**.
- Następnie tworzony jest rekord w tabeli `Wydatki` w AirTable.

### 💰 Dla wpływów:

- Analiza tytułu i kwoty przelewu.
- Tworzony jest rekord w tabeli `Przychody`.

<img width="931" height="384" alt="image" src="https://github.com/user-attachments/assets/6e860f3a-bae9-410f-bf6d-a837a739c49b" />


---

## 🧩 Faza 5: Komunikacja z CRM

- Na podstawie danych z przelewu, workflow wykonuje zapytanie do systemu CRM w celu:
  - Znalezienia pasującego zamówienia/faktury.
  - Zweryfikowania czy płatność została dokonana.
- Wspierane są różne scenariusze:
  - Numer zamówienia,
  - Numer faktury,
  - Numer zamówienia ze sklepu,
  - Płatność odroczona,
  - Brak identyfikatora – tylko dane nadawcy i kwota.

---

## 🔃 Faza 6: Aktualizacja statusu zamówienia

- Po dopasowaniu płatności do zamówienia, workflow aktualizuje status zamówienia na `opłacone`.

<img width="737" height="544" alt="image" src="https://github.com/user-attachments/assets/3fe20dbc-9428-4fa6-9681-3da5364e74af" />
<img width="1292" height="427" alt="image" src="https://github.com/user-attachments/assets/434b5447-8e8b-4b38-98e2-240e12b5586a" />

---

## 💡 Technologie i integracje

| Narzędzie        | Zastosowanie                                 |
|------------------|----------------------------------------------|
| Make.com         | Główne środowisko workflow                   |
| Webhook          | Odbieranie pliku CSV                         |
| AirTable         | Baza danych transakcji                       |
| OpenAI API       | Kategoryzacja wydatków, analiza tytułów przelewów                       |
| CRM (PlentyMarket) | Statusy zamówień i płatności             |

---
## **Autor**
Stworzony przez Rafał Zajkowski.
