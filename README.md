# 🧮 Vue 3 Calculator with Firebase Audit Logs

A lightweight calculator built with Vue 3 and styled using Tailwind CSS.
The app supports basic arithmetic operations and records each user action in Firebase Realtime Database as an audit log. Logs can be fetched and viewed directly from the interface.

---

## 🚀 Features

- Basic operations: addition, subtraction, multiplication, division
- Prevents invalid inputs (e.g., entering multiple operators in a row)
- Automatically copies the final result to the clipboard
- Logs all calculator actions to Firebase (numbers, operators, reset, result)
- Option to fetch and view audit logs
- Keyboard support:
- Enter → calculate
- Backspace → reset

---

## 🛠️ Tech Stack

- Vue 3
- [Tailwind CSS
- Firebase Realtime Database
- Bun(package manager)
- Vite (build tool)

---

## 📦 Project Setup

### 1️⃣ Clone the repository

```bash
git clone <your-repo-url>
cd vue3-calculator

```bash
bun install

#Firebase Setup
```bash
VITE_API_KEY=your_api_key
VITE_AUTH_DOMAIN=your_auth_domain
VITE_DB_URL=your_db_url
VITE_PROJECT_ID=your_project_id
VITE_STORAGE_BUCKET=your_storage_bucket
VITE_MSG_SENDER_ID=your_sender_id
VITE_APP_ID=your_app_id


#Run your server
```bash
bun dev



