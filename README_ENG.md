# 🧠 Automation of Bank Statement Analysis – Make.com + AirTable + CRM

## 📋 Project Description

The goal of this project is to fully automate the process of analyzing bank statements in CSV format, with further data integration into a CRM system and AirTable. The workflow is built in Make.com and is based on analyzing each incoming and outgoing transaction.

For each transaction:
- An appropriate record is created in AirTable.
- Expenses are categorized using the OpenAI API.
- Incomes are analyzed to identify payments for orders.
- Based on transfer data, the order status in the CRM is updated.

---

## 🗂️ Workflow Structure

The workflow consists of 6 main phases:

1. **Webhook – Receiving and processing the CSV file**
2. **Transaction segregation into incomes and expenses**
3. **Checking if the transaction exists in AirTable**
4. **Creating a record in AirTable and/or assigning a category**
5. **CRM Communication**
6. **Updating order status based on transfer data**

---

## 🔄 Phase 1: Webhook – Receiving and processing the CSV file

- **Trigger**: Make.com Webhook that receives the CSV file (e.g. email attachment, form upload).
- **Data processing**: The CSV is parsed – each row represents one transaction.
- **Data transformation**: Data is cleaned, the unnecessary first 4 lines and the last line of each file are removed, a CSV object is created, numbers are parsed as numeric values, commas are replaced with dots.

<img width="1192" height="97" alt="image" src="https://github.com/user-attachments/assets/0ad53d75-6738-4df0-a5bd-ac577beb3283" />


---

## 🔍 Phase 2: Transaction segregation – incomes and expenses

- Transactions are divided into:
  - **Incomes** – positive amount values.
  - **Expenses** – negative values.
- The decision is based on the sign of the amount or the transaction type.

<img width="508" height="457" alt="image" src="https://github.com/user-attachments/assets/c8128487-2f23-4c93-8485-1b6e704c56af" />
<img width="355" height="407" alt="image" src="https://github.com/user-attachments/assets/51731a04-24c3-4b23-99b5-11cf15b61422" />


---

## 🔁 Phase 3: Checking if the transaction exists in AirTable

- Each transaction is checked for duplicates in the AirTable database.
- Identified by:
  - Date,
  - Sender,
  - Transfer title,
  - Amount.

<img width="338" height="244" alt="image" src="https://github.com/user-attachments/assets/26814565-c3d5-4057-a01f-e59c5f3dadd4" /><img width="357" height="323" alt="image" src="https://github.com/user-attachments/assets/491c11f7-20fe-4bf7-a9f9-6d933c905cce" />



---

## 🧾 Phase 4: Transaction processing

### 💸 For expenses:

- Transaction data is sent to the OpenAI API (GPT-4) to assign an appropriate **expense category**.
- Then a record is created in the `Expenses` table in AirTable.

### 💰 For incomes:

- Analysis of transfer title, sender details, and amount.
- A record is created in the `Incomes` table.

<img width="931" height="384" alt="image" src="https://github.com/user-attachments/assets/6e860f3a-bae9-410f-bf6d-a837a739c49b" />


---

## 🧩 Phase 5: CRM Communication

- Based on transfer data, the workflow performs a query to the CRM system to:
  - Find the matching order/invoice.
  - Verify if the payment was made.
- Various scenarios are supported:
  - Order number,
  - Invoice number,
  - Online store order number,
  - Deferred payment,
  - No identifier – only sender and amount data.

---

## 🔃 Phase 6: Order status update

- After matching a payment to an order, the workflow updates the order status to `paid`.

<img width="737" height="544" alt="image" src="https://github.com/user-attachments/assets/3fe20dbc-9428-4fa6-9681-3da5364e74af" />
<img width="1292" height="427" alt="image" src="https://github.com/user-attachments/assets/434b5447-8e8b-4b38-98e2-240e12b5586a" />

---

## 💡 Technologies and Integrations

| Tool             | Purpose                                        |
|------------------|------------------------------------------------|
| Make.com         | Main workflow platform                         |
| Webhook          | Receiving the CSV file                         |
| AirTable         | Transaction data storage                       |
| OpenAI API       | Expense categorization, analysis of transfer titles                        |
| CRM (PlentyMarket) | Order and payment status management      |

---
## **Author**
Created by Rafał Zajkowski.
