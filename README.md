# Server-Driven UI: Frontend

This frontend is built using **Next.js 15** with TailwindCSS as the styling framework. The UI in this application is **completely controlled by the server**, where the frontend only fetches JSON from the backend and renders the UI according to the provided data.

## 🎯 Objectives
- Separate UI logic from the frontend so it can be updated without requiring redeployment.
- Enable **dynamic** UI changes by simply modifying the API response from the backend.
- Provide a flexible user experience integrated with **token-based authentication**.

## 🛠️ Technologies Used
- **Next.js 15** - React framework for full-stack applications.
- **TailwindCSS** - Utility-based styling for fast and consistent design.
- **Axios / fetch** - For fetching UI data from the backend.
- **LocalStorage** - Store authentication tokens for user sessions.

## 📌 Frontend Workflow
1. **Fetch UI from backend**
   When the page loads, the frontend will call the `/ui/:page` endpoint on the backend to get the UI structure in JSON format.

2. **Render UI based on JSON**
   The received JSON will be used to render components like input, button, and link **dynamically**.

3. **Interact with API**
   All buttons and inputs configured in the JSON will make requests to the backend according to the endpoints specified in the JSON.

4. **Authentication & Routing**
   - Login token is stored in `localStorage` after successful login.
   - If users don't have a token, they will be redirected back to the login page (`/auth`).

---

## 📂 Project Structure

### Frontend (Next.js)
```
frontend/

├── app/  --> application folder
│   ├── auth --> authentication folder for login
│   │   ├── page.tsx  --> login page
│   ├── dashboard/  --> dashboard page folder
│   │   ├── page.tsx --> dashboard page after successful login
│   ├── register/ --> register page folder
│   │   ├── page.tsx --> register page
│   ├── favicon.ico --> application icon
│   ├── globals.css --> global CSS styling for the application
│   ├── layout.tsx  --> application layout
│   ├── page.tsx  --> main application page
├── components/  --> application components folder
│   ├── ui/  --> UI application folder
│   │  ├── button.tsx --> reusable button component
│   │  ├── card.tsx  --> reusable card component
│   │  ├── input.tsx  --> reusable input component
│   │  ├── label.tsx  --> reusable label component
├── lib/ --> application lib folder
│   ├── utils.ts --> utils file for tailwind merge
├── utils/ --> application utils folder
│   ├── fetchUI.ts --> reusable fetchUI file
├── package.json --> package module list file for building and installing the application
```
---

## 🧭 Frontend Architecture

```
[User]
  ↓
[Next.js Page] ───────── fetch(`/ui/:endpoint`)
  ↓                                 ↓
[fetch-ui.ts]               [Backend UIController]
  ↓
[Render Dynamic UI Components]

Login/Register flow:
[Login/Register Page] ──── POST /auth/login
                          ──── POST /auth/register
                                    ↓
                             [Backend AuthModule]
                                      ↓
                                 [Supabase DB]

Dashboard flow:
[Dashboard Page] ───────── GET /auth/profile ─────▶ [Backend AuthModule]
                                      ↓
                                 [Supabase DB]
```

📌 **Endpoints accessed from Frontend to Backend:**

- `GET /ui/home` → Display main page
- `GET /ui/auth` → Display login form structure
- `GET /ui/register` → Display register form structure
- `POST /auth/login` → Process user login
- `POST /auth/register` → Process new user registration
- `GET /auth/profile` → Get profile data of currently logged in user

Explanation:
- The frontend is only responsible for displaying the UI that has been prepared by the backend in JSON format.
- Components are dynamically created based on the structure from the `/ui/:endpoint` endpoint.
- All interactions (login, register, profile) point to the backend.
- This architecture makes the frontend lightweight and flexible, as the backend is responsible for UI logic and design.


---

## 🚀 How to Run

### 2️⃣ Frontend (Next.js)
#### Install dependencies:
```sh
cd frontend
npm install
```

#### Run the frontend server:
```sh
npm run dev
```

Access the application at **http://localhost:3000**

---

## 🎨 Server-Driven UI: Example Response
Example response from the `/ui/home` endpoint - Backend sends JSON like this:
```json
{
  "title": "Authentication",
  "navTitle": "Server-Driven UI App",
  "navLinks": [
    { "text": "Home", "route": "/" },
    { "text": "Login", "route": "/auth" }
  ],
  "fields": [
    { "type": "input", "placeholder": "Username", "name": "username" },
    { "type": "input", "placeholder": "Password", "name": "password", "secure": true }
  ],
  "actions": [
    { "type": "button", "text": "Login", "endpoint": "/auth/login" },
    { "type": "button", "text": "Register", "endpoint": "/auth/register" }
  ]
}
```

The frontend will render the UI based on this data dynamically.

---

## 📢 Notes
- Use **Node.js 18+** to ensure compatibility.
- Make sure **backend and frontend** are running on the correct servers.

- Ensure the frontend fetches data with **fetchUI()** to match the JSON from the backend.

---

## 📜 License
This project is licensed under the MIT License.
