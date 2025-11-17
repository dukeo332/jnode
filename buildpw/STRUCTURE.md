# Project Structure - Visual Guide

## 📂 Complete File Tree

```
password-generator/
│
├── 📄 index.html              ← HTML entry point
├── 📄 package.json            ← Dependencies and scripts
├── 📄 vite.config.js          ← Network configuration for mint2025
│
├── 📁 src/                    ← Source code directory
│   ├── 📄 App.vue             ← Main component (password logic here)
│   ├── 📄 wordlist.js         ← **REPLACE THIS** with your 6000 words
│   ├── 📄 main.js             ← Vue app initialization
│   └── 📄 style.css           ← Global styles
│
├── 🔧 convert-wordlist.js     ← Helper script for wordlist conversion
│
└── 📚 Documentation/
    ├── 📘 OVERVIEW.md         ← Start here! Complete project overview
    ├── 📗 QUICKSTART.md       ← 5-minute setup guide
    ├── 📙 SUMMARY.md          ← What changed and why
    ├── 📕 README.md           ← Full documentation
    ├── 📔 ALGORITHM.md        ← Visual flow and examples
    └── 📋 CHECKLIST.md        ← Deployment checklist
```

## 🎯 File Purposes

### Configuration Files (Setup)

**package.json**
```json
{
  "dependencies": { "vue": "^3.4.0" },
  "scripts": {
    "dev": "vite",      ← Run this to start
    "build": "vite build"
  }
}
```
→ Defines what needs to be installed

**vite.config.js**
```javascript
{
  server: {
    host: true,              ← Allow network access
    allowedHosts: ['mint2025'] ← Your server hostname
  }
}
```
→ Makes app accessible on network

**index.html**
```html
<div id="app"></div>
<script src="/src/main.js"></script>
```
→ Loads the Vue app

### Source Files (The App)

**src/main.js** (Entry Point)
```javascript
import App from './App.vue'
createApp(App).mount('#app')
```
→ Initializes Vue and mounts App component

**src/App.vue** (Main Logic - 9 KB)
```vue
<template>
  <!-- Password display, button, details -->
</template>

<script>
  // Password generation algorithm
  // Copy function
  // Statistics tracking
</script>

<style>
  /* Component styling */
</style>
```
→ All password logic lives here

**src/wordlist.js** (Word Data - 1.1 KB → 150 KB)
```javascript
export const words = [
  'able',    ← Currently 100 words
  'about',   ← Will be 6000 words
  'above',
  // ...
]
```
→ **THIS IS THE FILE YOU'LL REPLACE**

**src/style.css** (Global Styles)
```css
* { box-sizing: border-box; }
body { font-family: sans-serif; }
```
→ Base styling for whole app

### Helper Files (Tools)

**convert-wordlist.js** (Conversion Script)
```javascript
// Reads: your-wordlist.txt
// Outputs: formatted JavaScript array
// Usage: node convert-wordlist.js input.txt > src/wordlist.js
```
→ Converts your text file to JS format

### Documentation Files (Guides)

```
OVERVIEW.md    → "What is this project?"
QUICKSTART.md  → "How do I get started?"
SUMMARY.md     → "What changed from before?"
README.md      → "Complete documentation"
ALGORITHM.md   → "How does it work?"
CHECKLIST.md   → "Step-by-step deployment"
```

## 🔄 Data Flow

```
User clicks "Generate" button
         │
         ▼
    App.vue calls generatePassword()
         │
         ▼
    Import words from wordlist.js
         │
         ▼
    Pick random components
    (words, number, symbols, position)
         │
         ▼
    Apply capitalization rules
         │
         ▼
    Assemble password string
         │
         ▼
    Check length (12-18?)
         │
         ├─ YES → Display password
         │         Update details
         │         Update statistics
         │
         └─ NO → Retry (loop back)
```

## 📦 What Gets Installed (node_modules)

When you run `npm install`, these get added:

```
node_modules/           ← Created by npm install
├── vue/               (~500 KB) - Vue.js framework
├── vite/              (~2 MB)   - Build tool
├── @vitejs/plugin-vue/           - Vue plugin for Vite
└── [dependencies]                - Various helper packages

Total: ~50-100 MB
```

**Note:** Don't upload node_modules to the server. Run `npm install` on the server instead.

## 🎨 Component Structure

```
App.vue
  │
  ├─ <template>
  │    │
  │    ├─ .password-card
  │    │    │
  │    │    ├─ <h1> Title
  │    │    │
  │    │    ├─ .password-display
  │    │    │    ├─ .password-text (the actual password)
  │    │    │    └─ .copy-btn (copy button)
  │    │    │
  │    │    ├─ .generate-btn (generate button)
  │    │    │
  │    │    ├─ .password-details (breakdown section)
  │    │    │    ├─ Words used
  │    │    │    ├─ Number used
  │    │    │    ├─ Symbols used
  │    │    │    ├─ Pattern
  │    │    │    └─ Length
  │    │    │
  │    │    └─ .stats (statistics section)
  │    │         ├─ Total generated
  │    │         ├─ Last attempts
  │    │         └─ Average attempts
  │    │
  │
  ├─ <script setup>
  │    │
  │    ├─ Import: { words } from './wordlist.js'
  │    ├─ Reactive state: password, details, stats
  │    ├─ Symbol arrays: symbolsNonShift, symbolsShift
  │    ├─ Function: generatePassword()
  │    └─ Function: copyPassword()
  │
  └─ <style scoped>
       └─ Component-specific CSS
```

## 🗂️ File Sizes

```
Configuration Files:
├── package.json        322 bytes
├── vite.config.js      220 bytes
└── index.html          365 bytes

Source Files:
├── main.js             111 bytes
├── style.css           301 bytes
├── App.vue             9.0 KB    ← Main component
└── wordlist.js         1.1 KB    ← Will grow to ~150 KB

Helper:
└── convert-wordlist.js 1.5 KB

Documentation:
├── OVERVIEW.md         ~12 KB
├── ALGORITHM.md        8.2 KB
├── CHECKLIST.md        5.3 KB
├── SUMMARY.md          4.3 KB
├── README.md           3.9 KB
└── QUICKSTART.md       2.9 KB

Total (without node_modules): ~50 KB
Total (with node_modules):    ~50-100 MB
Total (with full wordlist):   ~200 KB
```

## 📍 Key File Locations

**What you'll edit:**
```
src/wordlist.js          ← Replace with your 6000 words
```

**What you might customize:**
```
src/App.vue              ← Password logic, UI, styling
vite.config.js           ← Network settings, port
```

**What you won't need to change:**
```
src/main.js              ← Works as-is
src/style.css            ← Global styles (fine as-is)
index.html               ← HTML entry (fine as-is)
package.json             ← Dependencies (fine as-is)
```

## 🚀 Deployment Paths

### Development (Current)
```
Your Computer
    │
    └─ password-generator/  ← This folder
           └─ Upload to server →
```

### On Server (After Upload)
```
/home/mpstech/mynodes/
    │
    └─ password-generator/    ← Project root
           ├─ npm install here
           ├─ npm run dev here
           └─ Access: http://mint2025:5173
```

### Production (Optional)
```
/home/mpstech/mynodes/password-generator/
    │
    ├─ npm run build    ← Creates dist/
    │
    └─ dist/
         └─ Copy to → /var/www/html/password-gen/
                           └─ Access: http://mint2025/password-gen/
```

## 🎯 The One File You Must Change

```
📄 src/wordlist.js
```

**Current state:**
- 100 placeholder words
- ~1.1 KB file size
- Good for testing

**Future state:**
- 6000 real words
- ~150 KB file size  
- Production ready

**How to change it:**
```bash
node convert-wordlist.js your-list.txt > src/wordlist.js
```

That's it! Everything else stays the same.

## 📊 Development Workflow

```
1. Upload project → /home/mpstech/mynodes/password-generator/

2. Install dependencies:
   cd ~/mynodes/password-generator
   npm install

3. Test with placeholder:
   npm run dev
   → Open http://mint2025:5173
   → Test all features

4. Replace wordlist:
   node convert-wordlist.js my-6000-words.txt > src/wordlist.js

5. Test with full list:
   → Dev server auto-reloads
   → Verify everything works
   → Generate many passwords

6. Done! (or build for production)
```

---

**Ready to start?** → Open `QUICKSTART.md`  
**Need details?** → Open `README.md`  
**Want to understand the code?** → Open `src/App.vue`
