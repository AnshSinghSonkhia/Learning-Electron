# Learning Electron.js

Terminal Commands:

```sh
npm create vite .
```

- Select Framework: `React`
- Select Variant: `TypeScript`

```sh
npm i
npm run dev
```

## Create a `ui` folder in the `src` directory

- move everything from the `src` directory to `ui` folder.
- update all the imports in other files like: `index.html`.

## We don't need the `public` folder, that include `vite.svg`.

- Remove them

## Let's check our Build & dist stuff

```sh
npm run build
```

- Now, you get the `dist` folder.

## Changes in `vite.config.ts` file

- from this:

```config.ts
export default defineConfig({
  plugins: [react()],
})
```

- Change to this:

```config.ts
export default defineConfig({
  plugins: [react()],
  base: './',
  build: {
    outDir: 'dist-react',
  },
})
```

## Remove the `dist` folder

```sh
Remove-Item -Recurse -Force .\dist
```

## Build again

```sh
npm run build
```
- you got `dist-react` folder
- add it to `.gitignore`

# Install Electron

```sh
npm i --save-dev electron
```

- Add this in `package.json`

```json
"type": "module",
"main": "src/electron/main",
```

- Create folder `electron` in `src`.
- In it, create `main.js` file

- `src/electron/main.js` file

```js
import { app, BrowserWindow } from 'electron';
import path from 'path';

app.on('ready', () => {
    const mainWindow = new BrowserWindow({});
    mainWindow.loadFile(path.join(app.getAppPath(), '/dist-react/index.html'));
});
```

- updated script for `package.json`

```json
"scripts": {
    "dev:react": "vite",
    "dev:electron": "electron .",
    "build": "tsc -b && vite build",
    "lint": "eslint .",
    "preview": "vite preview",
    "transpile:electron": "tsc --project src/electron/tsconfig.json"
},
```

# Run the new `dev` command

```sh
npm run dev:electron
```

> It should open the electron app

# Add this to `tsconfig.app.json`

```json
"exclude": ["src/electron"],
```

- Create `tsconfig.json` file in `src/electron` folder.

> src/electron/tsconfig.json

```json
{
  "compilerOptions": {
    // require strict types (null-save)
    "strict": true,
    // tell TypeScript to generate ESM Syntax
    "target": "ESNext",
    // tell TypeScript to require ESM Syntax as input (including .js file imports)
    "module": "NodeNext",
    // define where to put generated JS
    "outDir": "../../dist-electron",
    // ignore errors from dependencies
    "skipLibCheck": true,
    // add global types
    "types": ["../../types"]
  }
}
```

- change it's `main.js` file to `main.ts` file.

> main.ts

```ts
import { app, BrowserWindow } from 'electron';
import path from 'path';

type test = string;

app.on('ready', () => {
    const mainWindow = new BrowserWindow({});
    mainWindow.loadFile(path.join(app.getAppPath(), '/dist-react/index.html'));
});
```

run

```sh
npm run transpile:electron
```

update `package.json`

```json
"main": "dist-electron/main",
```

run

```sh
npm run dev:electron
```



- Timestamp: 22:20