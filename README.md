# 🧮 Vue 3 Calculator with Firebase Audit Logs

A simple yet powerful calculator built using **Vue 3**, styled with **Tailwind CSS**, and integrated with **Firebase Realtime Database** to store and fetch audit logs of user actions.

---

## 🚀 Features

- Basic arithmetic operations: `+`, `-`, `×`, `÷`
- Prevents invalid inputs like multiple operators
- Automatically copies results to clipboard
- Logs every calculator event (number entered, operator used, reset, result displayed) to **Firebase Realtime Database**
- Fetch and view audit logs on demand
- Keyboard support (Enter = result, Backspace = reset)

---

## 🛠️ Tech Stack

- [Vue 3](https://vuejs.org/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Firebase Realtime Database](https://firebase.google.com/docs/database)
- [Bun](https://bun.sh/) (package manager)
- [Vite](https://vitejs.dev/) (build tool)

---

## 📦 Project Setup

### 1️⃣ Clone the repository

```bash
git clone <your-repo-url>
cd vue3-calculator
