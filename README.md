### 1. Initialize the project. `package.json`

```typescript
npm init
```

### 2. Install these dependencies: `Express`, `Mongoose`, `Eslint`, `Dotenv`, `Cors`

```typescript
yarn add express mongoose eslint dotenv cors
```

### 3. Install these dev dependencies: `Typescript`, `ts-node-dev`, `Prettier`, `Lint-Staged`, `Husky`, `eslint-config-prettier`

```typescript
yarn add -D typescript ts-node-dev prettier lint-staged husky eslint-config-prettier
```

### 4. Install `TypeScript definitions for Express`, `TypeScript definitions for cors`, `ESLint plugin`, `ESLint parser`

```typescript
yarn add -D @types/express @types/cors @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

---

### 5. Add a `tsconfig.json` file for `Typescript` configuration

```typescript
tsc --init
```

- Update `tsconfig.json`

```typescript
  "rootDir": "./src",
  "outDir": "./dist",
```

### 6. Update `package.json` scripts:

```typescript
"start": "ts-node-dev --respawn --transpile-only src/server.ts",
```

---

## Server Setup

### Create a `.env` file at root directory

```typescript
NODE_ENV = development
PORT = 5000
DATABASE_URL = // your-database-url
```

### Create a `.gitignore` file at root directory

```typescript
dist;
node_modules.env;
```

### Create a `config` folder inside `src` folder and inside `config` create a `index.ts` file.

### `src` > `config` > `index.ts`

```typescript
import dotenv from "dotenv";
import path from "path";

dotenv.config({ path: path.join(process.cwd(), ".env") });

export default {
  env: process.env.NODE_ENV,
  port: process.env.PORT || 5000,
  database_url: process.env.DATABASE_URL,
  default_user_pass: process.env.DEFAULT_USER_PASS,
};
```

### Create two files inside the `src` folder:

- `app.ts` (Express application setup)

```typescript
//app.ts
import express, { Application, Request, Response } from "express";
const app: Application = express();
import cors from "cors";

app.use(cors());

// parser
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

app.get("/", (req: Request, res: Response) => {
  res.send("Hello World!");
});

export default app;
```

- `server.ts` (Database connection setup)

```typescript
import mongoose from "mongoose";
import app from "./app";
import config from "./config";

async function main() {
  await mongoose.connect(config.database_url as string);
  console.log("Connected to MongoDB");
  app.listen(config.port, () => {
    console.log(`Server running at port ${config.port}`);
  });
}
main();
```

---

## Setup `ESLint`, `Prettier`, `Husky`, and `lint-staged`

### 1. Install the ESLint and Prettier extensions in your code editor.

### 2. Update `tsconfig.json`:

```json
"include": ["src"],
"exclude": ["node_modules"]
```

### 3. Install ESLint and TypeScript ESLint plugin:

```typescript
yarn add -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
npm install eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin --save-dev
```

### 4. Create `.eslintrc` file with the following content:

```json
{
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "plugins": ["@typescript-eslint"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "rules": {
    "no-unused-vars": "error",
    "no-console": "warn",
    "no-undef": "error",
    "no-unused-expressions": "error",
    "no-unreachable": "error",
    "@typescript-eslint/consistent-type-definitions": ["error", "type"]
  },
  "env": {
    "browser": true,
    "node": true,
    "es2021": true
  }
}
```

### 5. Add the following in package.json scripts:

```json
"lint:check": "eslint --ignore-path .eslintignore --ext .js,.ts .",
"lint:fix": "eslint . --fix"
```

### 6. Create .eslintignore file with the following content:

```
dist
node_modules
.env
```

### 7. Install Prettier:

```typescript
yarn add -D prettier
npm install prettier --save-dev
```

### 8. Create `.prettierrc` file with the following content:

```json
{
  "semi": true,
  "singleQuote": true,
  "arrowParens": "avoid"
}
```

### 9. Add the following in package.json scripts:

```json
"prettier:check": "prettier --ignore-path .gitignore --write \"**/*.+(js|ts|json)\"",
"prettier:fix": "prettier --write .",
```

### 10. Update settings.json in VS Code:

```json
"editor.defaultFormatter": "esbenp.prettier-vscode",
"editor.formatOnSave": true
```

### 11. Install eslint-config-prettier:

```typescript
yarn add -D eslint-config-prettier
npm install eslint-config-prettier --save-dev
```

### 12. Install Husky:

```typescript
yarn add husky --dev
npm install husky --save-dev
```

### 13. Install lint-staged:

```typescript
yarn add -D lint-staged
npm install lint-staged --save-dev
```

### 14. Update package.json scripts:

```json
"scripts": {
  //....other scripts
  "lint-prettier": "yarn lint:check && yarn prettier:check"
},
"lint-staged": {
  "src/**/*.ts": "yarn lint-prettier"
}
```

### 15. Update package.json scripts:

```typescript
yarn husky add .husky/pre-commit "yarn lint-staged"
npx husky add .husky/pre-commit "yarn lint-staged"
```

### Congratulations!🎉 You have successfully set up your Express backend project with ESLint, Prettier, Husky, and lint-staged.😃
