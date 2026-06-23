# create frontend
   ```bash
   npm create vite@latest frontend -- --template react-ts
   cd frontend
   npm install
   ```
install all dependencies and dev-dependencies.

### install and initialize tailwindcss
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
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   
   @layer base {
       body {
           @apply bg-gray-50 text-gray-900 antialiased;
       }
   }
   ```

# create backend
   ```bash
   mkdir backend
   cd backend
   npm init -y
   npm install express cors dotenv bcryptjs jsonwebtoken pg
   npm install -D typescript ts-node nodemon @types/express @types/cors @types/bcryptjs @types/jsonwebtoken @types/pg
   npx tsc --init
   cd ..
   ```

   Open backend/tsconfig.json and replace with:
   ```
   {
     "compilerOptions": {
       "target": "ES2022",
       "module": "commonjs",
       "esModuleInterop": true,
       "allowSyntheticDefaultImports": true,
       "strict": true,
       "outDir": "./dist",
       "rootDir": "./src",
       "declaration": true,
       "skipLibCheck": true,
       "forceConsistentCasingInFileNames": true,
       "resolveJsonModule": true,
       "allowImportingTsExtensions": true,
       "noEmit": true,
     },
     "include": ["src/**/*"],
     "exclude": ["node_modules", "dist"],
   }
   ```

   create `backend/src/server.ts`:
   ```
   import express from "express";
   import cors from "cors";
   import dotenv from "dotenv";
   
   dotenv.config();
   
   const app = express();
   const PORT = Number(process.env.PORT) || 3000;
   
   // Middleware
   app.use(
     cors({
       origin: "http://localhost:5173",
       credentials: true,
     }),
   );
   app.use(express.json());
   
   // Health check
   app.get("/", (req, res) => {
     res.json({ message: "Varaden API is running ✅" });
   });
   
   app.listen(PORT, () => {
     console.log(`Server is running on port ${PORT}`);
   });
   ```

   Open backend/package.json and update the scripts section:
   ```
   "scripts": {
     "dev": "nodemon --exec ts-node src/index.ts",
     "build": "tsc",
     "start": "node dist/index.js"
   }
   ```
   
   Create backend/nodemon.json:
   ```
   {
     "watch": ["src"],
     "ext": "ts",
     "exec": "ts-node src/index.ts"
   }
   ```
   


