# Quick Start Guide

## Step 1: Upload to Your Server

Upload the entire `password-generator` folder to your development server at:
```
/home/mpstech/mynodes/password-generator/
```

## Step 2: Install Dependencies

SSH into your server and run:
```bash
cd ~/mynodes/password-generator
npm install
```

## Step 3: Test with Placeholder List

Start the development server:
```bash
npm run dev
```

Access it at: `http://mint2025:5173`

The app will work with the 100-word placeholder list. Test all features:
- ✓ Generate password button
- ✓ Copy to clipboard
- ✓ View generation details
- ✓ Check statistics

## Step 4: Replace Wordlist (When Ready)

### Option A: Manual Replacement
1. Open your 6000-word text file
2. Format as JavaScript array (each word in quotes, separated by commas)
3. Replace the array in `src/wordlist.js`

### Option B: Use the Conversion Script
1. Copy your wordlist file to the project directory
2. Run the converter:
   ```bash
   node convert-wordlist.js your-wordlist.txt > src/wordlist.js
   ```
3. The script will create a properly formatted `wordlist.js` file

## Step 5: Verify Final Version

After replacing the wordlist:
1. The dev server will automatically reload
2. Generate a few passwords to verify
3. Check statistics - should see various attempt counts
4. All passwords should be 12-18 characters

## Troubleshooting

**Server won't start:**
- Check Node.js version: `node --version` (should be 20.x)
- Reinstall dependencies: `rm -rf node_modules && npm install`

**Can't access from network:**
- Verify `vite.config.js` has `host: true` and `allowedHosts: ['mint2025']`
- Check firewall allows port 5173

**Passwords too long/short:**
- This shouldn't happen with the retry loop
- If it does, check wordlist for unusually long words

## File Structure Reference

```
password-generator/
├── src/
│   ├── App.vue         ← Main component (password logic)
│   ├── wordlist.js     ← REPLACE THIS with your full list
│   ├── main.js         ← App initialization
│   └── style.css       ← Global styles
├── index.html          ← HTML entry
├── vite.config.js      ← Network config
├── package.json        ← Dependencies
├── convert-wordlist.js ← Helper script
└── README.md           ← Full documentation
```

## Key Features to Test

1. **Password Generation**: Click "Generate Password"
2. **Copy Function**: Click "Copy" button
3. **Details Display**: See words, number, symbols, pattern
4. **Statistics**: Track attempts and averages
5. **Responsive Design**: Test on different screen sizes

## Next Steps

Once you're happy with the app:
- Build for production: `npm run build`
- Deploy the `dist/` folder to Apache if needed
- Consider adding features like:
  - Password length preferences
  - Exclude certain symbols option
  - Password history
  - Save favorites
