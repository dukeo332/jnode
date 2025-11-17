# Password Generator

A Vue.js application that generates secure passwords using a word-based algorithm.

## Algorithm

The password generator creates passwords with the following characteristics:
- **Length**: 12-18 characters
- **Components**: 2 words + 1 number (1-3 digits) + 2 symbols
- **Pattern**: Number placement determines capitalization
  - Number at beginning: Capitalize first word only
  - Number in middle: Capitalize both words
  - Number at end: Capitalize second word only
- **Symbols**: Both symbols come from the same set (either non-shift or shift keys)

## Setup Instructions

### Initial Setup

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Run development server:**
   ```bash
   npm run dev
   ```

3. **Access the application:**
   - Local: `http://localhost:5173`
   - Network: `http://mint2025:5173` or `http://10.152.1.72:5173`

### Replacing the Placeholder Wordlist

The application currently uses a placeholder wordlist of ~100 words for development. To use your full 6000-word list:

1. **Prepare your wordlist file:**
   - Ensure it's a text file with one word per line
   - Clean up any extra whitespace or empty lines

2. **Convert to JavaScript array:**
   
   You can do this manually or use a simple script. Here's a quick way using command line:
   
   ```bash
   # If your wordlist is named 'fullwordlist.txt'
   awk '{printf "  '\''%s'\'',\n", $0}' fullwordlist.txt
   ```
   
   Or use this Node.js script (save as convert-wordlist.js):
   ```javascript
   const fs = require('fs');
   const words = fs.readFileSync('fullwordlist.txt', 'utf8')
     .split('\n')
     .filter(word => word.trim())
     .map(word => `  '${word.trim()}'`)
     .join(',\n');
   
   console.log('export const words = [\n' + words + '\n]');
   ```
   
   Run it:
   ```bash
   node convert-wordlist.js > src/wordlist.js
   ```

3. **Replace src/wordlist.js:**
   - Keep the same structure: `export const words = [...]`
   - Replace the placeholder array with your full 6000-word array

4. **Test:**
   - The application will automatically use the new wordlist
   - No other code changes needed
   - The retry loop will handle finding valid combinations

## Project Structure

```
password-generator/
├── index.html              # HTML entry point
├── package.json            # Dependencies and scripts
├── vite.config.js          # Vite configuration (network access)
├── src/
│   ├── main.js            # Vue app initialization
│   ├── App.vue            # Main component with password logic
│   ├── style.css          # Global styles
│   └── wordlist.js        # Word list (REPLACE THIS with your full list)
└── README.md              # This file
```

## Features

- **Random password generation** with configurable algorithm
- **Copy to clipboard** functionality
- **Generation details** showing words, numbers, symbols, and pattern used
- **Statistics** tracking attempts and efficiency
- **Responsive design** works on desktop and mobile
- **Retry logic** automatically finds valid password combinations

## Development Notes

### Why Use a Placeholder List?

- Faster development and testing
- Avoids processing large files during development
- Easy to swap in the full list when ready

### Retry Logic

The app uses a simple while loop that:
1. Picks 2 random words
2. Generates a random number (1-3 digits)
3. Selects symbols and applies capitalization rules
4. Checks if total length is 12-18 characters
5. If not, tries again (tracks attempts for statistics)
6. Safety limit of 1000 attempts (should never reach this)

With a diverse wordlist, most passwords generate in 1-5 attempts.

## Building for Production

```bash
npm run build
```

This creates a `dist/` folder with optimized files ready to deploy.

To preview the production build:
```bash
npm run preview
```

## License

Internal use for MPS Technology Department.
