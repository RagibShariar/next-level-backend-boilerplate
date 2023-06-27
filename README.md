## Lets setup Express Backend boilerplate with `ESLint`, `Prettier`, `Husky`, and `Lint-Staged` for our Typescript project.

## **Installation**

### 1. Initialize the project. `package.json`

```typescript
npm init
```

### 2. Create `node_modules`

```bash
yarn install
```

### 3. Install these dependencies: `Express`, `Mongoose`, `Eslint`, `Dotenv`, `Cors`

```typescript
yarn add express mongoose eslint dotenv cors
```

### 4. Install these dev dependencies: `Typescript`, `ts-node-dev`, `Prettier`, `Lint-Staged`, `Husky`, `eslint-config-prettier`

```typescript
yarn add -D typescript ts-node-dev prettier lint-staged husky eslint-config-prettier
```

### 5. Install `TypeScript definitions for Express`, `TypeScript definitions for cors`, `ESLint plugin`, `ESLint parser`

```typescript
yarn add -D @types/express @types/cors @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

### 6. Add a `tsconfig.json` file for `Typescript` configuration

```typescript
tsc --init
```

- Update `tsconfig.json`

```typescript
  "rootDir": "./src",
  "outDir": "./dist",
```

### 7. Update `package.json`

```typescript
"start": "ts-node-dev --respawn --transpile-only src/server.ts",
```

---

## **Server Setup**

### Create a `.env` file at root directory

```typescript
NODE_ENV = development
PORT = 5000
DATABASE_URL = mongodb+srv://<username>:<password>@cluster0.9nrwj.mongodb.net/database_name?retryWrites=true&w=majority

// replace <username>:<password>
```

### Create a `.gitignore` file at root directory

```typescript
dist;
node_modules;
.env;
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

### Create two files inside the `src` folder: `app.ts` and `server.ts`

- `app.ts` (Express application setup)

```typescript
//app.ts
import cors from "cors";
import express, { Application, Request, Response } from "express";

const app: Application = express();
// const port = 5000; // moved to src/config/index.ts

// Cors
app.use(cors());

// parser
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Testing
app.get("/", (req: Request, res: Response) => {
  res.send("Welcome to Digital Cow Hut 2023");
});

export default app;
```

- `server.ts` (Database connection setup)

```typescript
import { Server } from "http";
import mongoose from "mongoose";
import app from "./app";
import config from "./config";

// Uncaught exceptions handle
process.on("uncaughtException", (error) => {
  console.log(error);
  process.exit(1);
});

let server: Server;

async function bootstrap() {
  try {
    await mongoose.connect(config.database_url as string); // config/index.ts theke ashbe
    console.log(`🔗 Database connected successfully`);

    server = app.listen(config.port, () => {
      console.log(`💻 App is listening on port ${config.port}`);
    });
  } catch (err) {
    console.log(`Failed to connect`, err);
  }

  // Unhandled Rejection handle
  process.on("unhandledRejection", (error) => {
    // console.log(
    //   'Unhandled Rejection is detected, We are closing the server...'
    // );
    if (server) {
      server.close(() => {
        console.log(error);
        process.exit(1);
      });
    } else {
      process.exit(1);
    }
  });
}

bootstrap();

// SIGTERM handle
process.on("SIGTERM", () => {
  console.log("SIGTERM is received");
  if (server) {
    server.close();
  }
});
```

---

# Setup `ESLint`, `Prettier`, `Husky`, and `lint-staged`

### 1. Install the `ESLint` and `Prettier` extensions in your code editor.

### 2. Update `tsconfig.json`:

```json
{
  "include": ["src"], // which files to compile
  "exclude": ["node_modules"] // which files to skip
}
```

### 4. Create `.eslintrc` file at `root directory` with the following content:

- **[ESlint-Prettier Blog](https://blog.logrocket.com/linting-typescript-eslint-prettier/)**

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
    "es2021": true
    "node": true,
  },
  "globals": {
    "process": "readonly"
  }
}
```

### 5. Add the followings in `package.json`:

```json
"scripts": {
  "lint:check": "eslint --ignore-path .eslintignore --ext .js,.ts .",
  "lint:fix": "eslint . --fix",
  "prettier:check": "prettier --ignore-path .gitignore --write \"**/*.+(js|ts|json)\"",
  "prettier:fix": "prettier --write .",
  "lint-prettier": "yarn lint:check && yarn prettier:check",
},
"lint-staged": {
  "src/**/*.ts": "yarn lint-prettier"
},
```

### 6. Create `.eslintignore` file at `root directory` with the following content:

```
node_modules
dist
.env
```

### 8. Create `.prettierrc` file at `root directory` with the following content:

```json
{
  "semi": true,
  "singleQuote": true,
  "arrowParens": "avoid"
}
```

### initialize a `.husky` folder with the following command:

- **[Husky GitHub](https://typicode.github.io/husky/getting-started.html)**

```typescript
yarn husky install
```

### 15. Make a hook for husky pre-commit

```typescript
yarn husky add .husky/pre-commit "yarn lint-staged"
```

### Go to `.husky` folder and add the following command to `Pre-commit` file

```typescript
yarn lint-staged
```

---

### Congratulations!🎉 You have successfully set up your Express backend project with ESLint, Prettier, Husky, and lint-staged.😃
