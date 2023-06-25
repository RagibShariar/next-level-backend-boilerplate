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
