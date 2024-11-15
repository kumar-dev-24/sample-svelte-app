# Svelte.js Project
 A modern web application built using Svelte.js, a framework for building fast and interactive user interfaces.

## About This App
This app is designed to provide users with a seamless and interactive experience for managing tasks. It features an OTP-based login for secure authentication, allowing users to create and manage personal tasks. Users can also enjoy light and dark theme modes, and real-time UI updates with Svelte's reactive state management.

# Features
- Reactive Framework: Effortlessly manage UI state with Svelte's reactive features.
- Lightweight and Fast: No virtual DOM, no overhead – Svelte compiles to efficient JavaScript.
- Customizable Components: Modular, reusable components for building dynamic UIs.
- Optimized Development: Built-in support for animations, scoped styles, and transitions.

# Getting Started
`Prerequisites`
- Node.js (16+ recommended)
- A package manager like npm or yarn

# Installation
`Clone the repository:`
git clone https://github.com/yourusername/your-svelte-project.git
cd your-svelte-project

# Install dependencies:
- npm install
# or
- yarn

# Start the development server:
- npm run dev
# or
- yarn dev

# Open the app in your browser:
http://localhost:5173

# Available Scripts
Development: Start a development server with hot module reloading
- npm run dev
# Build: Create a production build of the app.
- npm run build
Preview: Preview the production build locally.
- npm run preview

# Customization

`Add New Pages:` Add a .svelte file in the routes/ directory and define the route in your router (if using a router).
`Styling:` Use scoped CSS in each .svelte file or include global styles in src/styles.
`State Management:` Use Svelte stores to manage shared application state.

# Deployment

To deploy your app:
- npm run build
Deploy the files in the build/ folder to your web server or hosting platform (e.g., Netlify, Vercel).

# About the App
- This application is built using Svelte and features a beautifully animated UI for OTP authentication. It provides a secure and seamless login experience with smooth transitions and real-time feedback.

# Key Highlights:

`OTP Authentication:` Users can log in using a One-Time Password.
`Testing OTP:` Use the code 123456 to test the functionality during development.
`Modern Animations:` The UI includes sleek animations for an engaging user experience.
`Responsive Design:` Ensures compatibility across various devices.

# License
This project is licensed under the MIT License.