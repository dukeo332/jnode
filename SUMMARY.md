# Password Generator - Simplified Version

## What Changed

### ✅ Simplified Algorithm
**Before:** Complex length-categorized wordlist with combination calculations
**After:** Single wordlist with simple retry loop

**Benefits:**
- Much easier to maintain
- No need to sort words by length
- Simple logic: pick 2 words, check length, retry if needed
- Your 6000-word list stays as-is (one word per line)

### ✅ Placeholder Wordlist Approach
**Development:** 100-word placeholder list included
**Production:** Simple swap to your full 6000-word list

**Why?**
- Faster development and testing
- No processing huge files during development
- Easy one-file replacement when ready

### ✅ Retry Loop Logic

The app now works like this:

```
1. Pick 2 random words
2. Generate random number (1-3 digits)
3. Pick 2 matching symbols
4. Apply capitalization based on number position
5. Assemble password
6. Check: Is it 12-18 characters?
   ├─ YES → Done! Show password
   └─ NO  → Try again (loop back to step 1)
```

**Statistics track:**
- How many attempts the last password took
- Average attempts across all generated passwords
- Total passwords generated

## Password Algorithm (Same as Before)

### Components
- 2 random words
- 1 number (1-3 digits, random)
- 2 symbols (both from same set: either regular or shift keys)

### Number Placement & Capitalization
```
Position 0 (Beginning):  123-Word-word
Position 1 (Middle):     Word-123-Word
Position 2 (End):        word-Word-123
```

### Symbol Sets
- Regular: ` - = [ ] \ ; ' , . /
- Shift: ~ ! @ # $ % ^ & * ( ) _ + { } | : " < > ?

## File Structure

```
password-generator/
├── src/
│   ├── App.vue          Main component with all logic
│   ├── wordlist.js      ← YOUR 6000 WORDS GO HERE
│   ├── main.js          Vue initialization
│   └── style.css        Global styles
├── index.html           HTML entry point
├── vite.config.js       Network configuration
├── package.json         Dependencies
├── convert-wordlist.js  Helper to convert your text file
├── README.md            Full documentation
└── QUICKSTART.md        Quick setup guide
```

## How to Use Your Full Wordlist

### Your Current File Format
```
able
about
above
across
...
(6000 words total)
```

### Target Format (JavaScript)
```javascript
export const words = [
  'able',
  'about',
  'above',
  'across',
  ...
]
```

### Conversion Options

**Option 1: Use the Provided Script**
```bash
node convert-wordlist.js your-wordlist.txt > src/wordlist.js
```

**Option 2: Manual (if you prefer)**
1. Open your wordlist in a text editor
2. Add quotes around each word: `'word'`
3. Add commas between words
4. Wrap in: `export const words = [ ... ]`

## Features Included

✅ Random password generation
✅ Copy to clipboard
✅ Generation details breakdown
✅ Statistics tracking
✅ Responsive design
✅ Network access configured for mint2025
✅ Modern Vue.js 3 with Composition API
✅ Clean, maintainable code

## Testing Checklist

With placeholder list (100 words):
- [ ] App loads at http://mint2025:5173
- [ ] Generate button creates password
- [ ] Password is 12-18 characters
- [ ] Copy button works
- [ ] Details show correctly
- [ ] Statistics update

With full list (6000 words):
- [ ] Replace src/wordlist.js
- [ ] App still loads (may take moment on first load)
- [ ] Generate multiple passwords
- [ ] Verify variety in words used
- [ ] Check attempt statistics (should be low, 1-5 usually)

## Expected Performance

With 6000 words:
- **Average attempts per password:** 1-5
- **Generation time:** Instant (<1ms)
- **Success rate:** ~100% (safety limit of 1000 attempts)

The more diverse your wordlist, the faster it finds valid combinations.

## What's Next?

1. Upload to your server: `/home/mpstech/mynodes/password-generator/`
2. Run `npm install`
3. Test with placeholder: `npm run dev`
4. When ready, swap in your full wordlist
5. Build for production: `npm run build` (if needed)

## Support Files Included

- **README.md** - Complete documentation
- **QUICKSTART.md** - Step-by-step setup guide
- **convert-wordlist.js** - Wordlist conversion script
- **All source files** - Ready to run

---

**Questions?** Check the README.md for detailed information or QUICKSTART.md for quick setup steps.
