# Electron App Setup and Build Guide

## 1. Running the App
To start the Electron app in development mode, use:
```bash
npm start
```

## 2. Setting Up for Production Build
To build the app for production, follow these steps:

1. **Install `electron-builder`:**
   ```bash
   npm install electron-builder --save-dev
   ```

2. **Update `package.json` with the following configurations:**
   ```json
   {
     "name": "my-electron-app",
     "version": "1.0.0",
     "description": "My Electron Application",
     "main": "main.js",
     "scripts": {
       "start": "electron .",
       "build": "electron-builder"
     },
     "devDependencies": {
       "electron": "^25.0.0",
       "electron-builder": "^24.6.0"
     },
     "build": {
       "appId": "com.myapp.electron",
       "productName": "MyElectronApp",
       "win": {
         "target": "nsis",
         "icon": "build/icon.ico"
       }
     }
   }
   ```

   ### Important: Disabling Windows Code Signing
   To disable code signing for Windows builds, add the following to the `build` section in `package.json`:
   ```json
   "build": {
     "win": {
       "sign": false
     }
   }
   ```

3. **Build the App:**
   Run the following command to package your Electron app:
   ```bash
   npm run build
   ```

## 3. Boilerplate Code for `main.js`
Use the following boilerplate code for setting up the main Electron process:

```javascript
const { app, BrowserWindow } = require('electron');

function createWindow() {
    const win = new BrowserWindow({
        width: 800,
        height: 600,
        webPreferences: {
            nodeIntegration: true,
        },
    });

    // Load the main HTML file
    win.loadFile('index.html');

    // Remove the default menu bar
    win.removeMenu();
}

// Initialize the app
app.whenReady().then(createWindow);

// Close the app when all windows are closed (except on macOS)
app.on('window-all-closed', () => {
    if (process.platform !== 'darwin') {
        app.quit();
    }
});

// Reopen the app if the icon is clicked in the dock (macOS behavior)
app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) {
        createWindow();
    }
});
```

## 4. Additional Notes
- **Adding a `start` Script**: Ensure the `start` script is in `package.json`:
   ```json
   "scripts": {
     "start": "electron ."
   }
   ```
