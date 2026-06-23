1. npm create vite@latest frontend -- --template react-ts
2. cd frontend
3. npm install
4. install all dependencies and dev-dependencies
5. install and initialize tailwindcss
   ```bash
   npm install -D tailwindcss@3 postcss autoprefixer
   npx tailwindcss init -p
   ```

   `tailwind.config.js` — replace with:

   ```javascript
   export default {
     content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
     theme: {
       extend: {},
     },
     plugins: [],
   }
   ```

   `src/index.css` — replace everything with:

   ```
   css@tailwind base;
   @tailwind components;
   @tailwind utilities;6.
   ```
