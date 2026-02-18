Starter React Vitest Stryker

Steps to reproduce the repository

```bash
node -v
v22.12.0
```

```bash
npm -v
11.10.0
```

`npm create vite@latest`

```bash
npm create vite@latest

> npx
> create-vite


To create in one go, run: create-vite <DIRECTORY> --no-interactive --template <TEMPLATE>

│
◇  Project name:
│  starter-react-vitest-stryker
│
◇  Select a framework:
│  React
│
◇  Select a variant:
│  TypeScript
│
◇  Use Vite 8 beta (Experimental)?:
│  No
│
◇  Install with npm and start now?
│  Yes
│
◇  Scaffolding project in C:\Users\thoma\projects\perso\tests_sandbox\starter-react-vitest-stryker...
│
◇  Installing dependencies with npm...
npm warn EBADENGINE Unsupported engine {
npm warn EBADENGINE   package: 'eslint-visitor-keys@5.0.0',
npm warn EBADENGINE   required: { node: '^20.19.0 || ^22.13.0 || >=24' },
npm warn EBADENGINE   current: { node: 'v22.12.0', npm: '11.10.0' }
npm warn EBADENGINE }

added 177 packages, and audited 178 packages in 7s

46 packages are looking for funding
  run `npm fund` for details

10 moderate severity vulnerabilities

To address issues that do not require attention, run:
  npm audit fix

To address all issues possible (including breaking changes), run:
  npm audit fix --force

Some issues need review, and may require choosing
a different dependency.

Run `npm audit` for details.
│
◇  Starting dev server...

> starter-react-vitest-stryker@0.0.0 dev
> vite


  VITE v7.3.1  ready in 641 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

`npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom`

Edit **package.json** in root folder
```json
{
  "name": "starter-react-vitest-stryker",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "test": "vitest", // 🚨
    "build": "tsc -b && vite build",
    "lint": "eslint .",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^19.2.0",
    "react-dom": "^19.2.0"
  },
  "devDependencies": {
    "@eslint/js": "^9.39.1",
    "@testing-library/jest-dom": "^6.9.1",
    "@testing-library/react": "^16.3.2",
    "@types/node": "^24.10.1",
    "@types/react": "^19.2.7",
    "@types/react-dom": "^19.2.3",
    "@vitejs/plugin-react": "^5.1.1",
    "eslint": "^9.39.1",
    "eslint-plugin-react-hooks": "^7.0.1",
    "eslint-plugin-react-refresh": "^0.4.24",
    "globals": "^16.5.0",
    "jsdom": "^28.1.0",
    "typescript": "~5.9.3",
    "typescript-eslint": "^8.48.0",
    "vite": "^7.3.1",
    "vitest": "^4.0.18"
  }
}

```

Create the **vitest.config.ts** in root folder
```ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
  },
})
```

Create the file **App.test.tsx** in src folder
```tsx
import { render, screen, fireEvent } from '@testing-library/react'
import App from './App'
import { test, expect } from 'vitest'
import { vi } from 'vitest'

// Mock static assets to prevent path errors
vi.mock('/vite.svg', () => ({ default: 'vite-logo' }))
vi.mock('/react.svg', () => ({ default: 'react-logo' }))

test('renders title and initial counter', () => {
  render(<App />)
  expect(screen.getByText(/Vite \+ React/i)).toBeDefined()
  expect(screen.getByRole('button').textContent).toBe('count is 0')
})

test('increments counter on click', () => {
  render(<App />)
  const button = screen.getByRole('button')

  fireEvent.click(button)

  expect(button.textContent).toBe('count is 1')
})
```