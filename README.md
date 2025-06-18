

# 💰 Svelte Expense Tracker

A simple and responsive expense tracker application built with **Svelte** and **Appwrite**. This app allows users to register, log in, and manage their expenses efficiently.

## 🚀 Live Demo

🔗 [Visit Expense Tracker](https://github.com/chiragbhoi01/Chirag-Portfolio/blob/main/README.md)

## 🛠️ Features

* 🔐 **User Authentication**: Register and log in securely.
* ➕ **Expense Management**: Add, delete, and manage expenses with ease.
* 🖥️ **Responsive Design**: Works seamlessly on all devices.
* 🗃️ **Backend with Appwrite**: Handles user data and expenses efficiently.
* 💡 **Simple UI**: Clean and intuitive interface.

## 🧰 Tech Stack

* **Frontend**: Svelte, CSS
* **Backend**: Appwrite
* **Deployment**: Netlify (or any other platform)
* **Other Tools**: Node.js, Vite

## 🧑‍💻 How to Use

### Prerequisites

* Node.js installed on your machine.
* An Appwrite instance set up (either self-hosted or on Appwrite Cloud).

### Steps to Run Locally

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/svelte-expense-tracker.git
   cd svelte-expense-tracker
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Set up environment variables:

   * Create a `.env` file in the root directory.
   * Add your Appwrite endpoint and project details:

     ```env
     VITE_APPWRITE_ENDPOINT=https://<YOUR_APPWRITE_ENDPOINT>
     VITE_APPWRITE_PROJECT=<YOUR_APPWRITE_PROJECT_ID>
     ```

4. Run the development server:

   ```bash
   npm run dev
   ```

5. Open [http://localhost:5173](http://localhost:5173) in your browser.

## 📁 Project Structure

```
Svelte-Expense-Tracker/
│
├── public/             # Static assets
├── src/
│   ├── components/     # Reusable components
│   ├── pages/          # App pages (e.g., login, dashboard)
│   ├── App.svelte      # Main app component
│   └── main.js         # Entry point
├── .env                # Environment variables
├── README.md           # Project overview
└── rollup.config.js    # Build configuration
```

## 📌 Future Enhancements

* Add expense categories.
* Implement data visualization for expenses.
* Enable export/import of expense data.
* Introduce recurring expense tracking.

## 🙋‍♂️ Author

**Your Name**
📍 Your Location
📧 [your-email@example.com](mailto:your-email@example.com)
🔗 [LinkedIn](https://www.linkedin.com/in/your-profile)
🔗 [GitHub](https://github.com/your-username)

