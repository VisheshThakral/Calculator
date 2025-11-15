# 🧮 Vue 3 Calculator with Firebase Audit Logs

A lightweight calculator built with **Vue 3** and styled using **Tailwind CSS**.  
The application supports basic arithmetic operations and records each user action in **Firebase Realtime Database** as an audit log. You can also fetch and view these logs from the interface.

---

## 🚀 Features

- Basic arithmetic: add, subtract, multiply, divide  
- Prevents invalid inputs (e.g., consecutive operators)  
- Automatically copies the calculated result to the clipboard  
- Logs all calculator actions to Firebase:
  - number inputs  
  - operator usage  
  - reset action  
  - result calculation  
- Fetch and view audit logs on demand  
- Keyboard shortcuts:
  - **Enter** → calculate  
  - **Backspace** → reset  

---

## 🛠️ Tech Stack

- **Vue 3**  
- **Tailwind CSS**  
- **Firebase Realtime Database**  
- **Bun** (package manager)  
- **Vite** (build tool)

---

## 📦 Project Setup

### 1️⃣ Clone the repository

```bash
git clone https://github.com/VisheshThakral/Calculator
cd calculator
