# Password Generation Algorithm - Visual Flow

## Step-by-Step Process

```
┌─────────────────────────────────────────────────────────────┐
│  START: Generate Password                                   │
└────────────────┬────────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 1: Pick Random Components                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ • Select 2 random words from wordlist                │   │
│  │ • Generate random number (1-3 digits)                │   │
│  │ • Choose symbol set (regular OR shift)               │   │
│  │ • Pick 2 random symbols from chosen set              │   │
│  │ • Randomly pick number position (0, 1, or 2)         │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────────┬────────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 2: Apply Capitalization Rules                         │
│                                                              │
│  IF number_position = 0 (beginning):                        │
│      Word1 = Capitalize                                     │
│      Word2 = lowercase                                      │
│                                                              │
│  IF number_position = 1 (middle):                           │
│      Word1 = Capitalize                                     │
│      Word2 = Capitalize                                     │
│                                                              │
│  IF number_position = 2 (end):                              │
│      Word1 = lowercase                                      │
│      Word2 = Capitalize                                     │
└────────────────┬────────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 3: Assemble Password                                  │
│                                                              │
│  IF number_position = 0:                                    │
│      password = number + symbol1 + Word1 + symbol2 + Word2  │
│      Example: 42-Apple-banana                               │
│                                                              │
│  IF number_position = 1:                                    │
│      password = Word1 + symbol1 + number + symbol2 + Word2  │
│      Example: Apple-42-Banana                               │
│                                                              │
│  IF number_position = 2:                                    │
│      password = Word1 + symbol1 + Word2 + symbol2 + number  │
│      Example: apple-Banana-42                               │
└────────────────┬────────────────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────────────────┐
│  STEP 4: Length Check                                       │
│                                                              │
│  Is password length between 12 and 18 characters?           │
│                                                              │
│      YES ────────────► Use this password! ─────► DONE      │
│                                                              │
│      NO ─────► Increment attempt counter                    │
│                │                                             │
│                ▼                                             │
│           Loop back to STEP 1                               │
│           (Safety limit: 1000 attempts)                     │
└─────────────────────────────────────────────────────────────┘
```

## Concrete Examples

### Example 1: Number at Beginning (Position 0)
```
Components:
  word1 = "apple"
  word2 = "banana"
  number = 42 (2 digits)
  symbols = ["-", "-"] (from regular set)
  position = 0

Capitalization:
  word1 → "Apple" (capitalize first word)
  word2 → "banana" (keep lowercase)

Assembly:
  42-Apple-banana

Length Check:
  4 + 2 + 5 + 1 + 6 = 15 characters ✓ (12-18 range)
  
Result: "42-Apple-banana"
```

### Example 2: Number in Middle (Position 1)
```
Components:
  word1 = "hello"
  word2 = "world"
  number = 7 (1 digit)
  symbols = ["!", "!"] (from shift set)
  position = 1

Capitalization:
  word1 → "Hello" (capitalize first word)
  word2 → "World" (capitalize second word)

Assembly:
  Hello!7!World

Length Check:
  5 + 1 + 1 + 1 + 5 = 13 characters ✓ (12-18 range)
  
Result: "Hello!7!World"
```

### Example 3: Number at End (Position 2)
```
Components:
  word1 = "tiger"
  word2 = "mountain"
  number = 123 (3 digits)
  symbols = ["@", "@"] (from shift set)
  position = 2

Capitalization:
  word1 → "tiger" (keep lowercase)
  word2 → "Mountain" (capitalize second word)

Assembly:
  tiger@Mountain@123

Length Check:
  5 + 1 + 8 + 1 + 3 = 18 characters ✓ (12-18 range)
  
Result: "tiger@Mountain@123"
```

### Example 4: Too Short - Retry
```
Components:
  word1 = "cat"
  word2 = "dog"
  number = 5 (1 digit)
  symbols = ["-", "-"]
  position = 1

Assembly:
  Cat-5-Dog

Length Check:
  3 + 1 + 1 + 1 + 3 = 9 characters ✗ (too short, need 12-18)
  
Action: Attempt counter++, loop back to STEP 1
```

## Symbol Sets Reference

### Regular Keys (Non-Shift)
```
` - = [ ] \ ; ' , . /
```
Example: apple-123-Banana

### Shift Keys
```
~ ! @ # $ % ^ & * ( ) _ + { } | : " < > ?
```
Example: Apple@123@banana

## Statistics Tracking

The app tracks:
- **Total Generated**: How many passwords created in session
- **Last Attempts**: How many tries for the most recent password
- **Average Attempts**: Mean attempts across all passwords
- **Total Attempts**: Cumulative retry count

### Expected Performance
With a diverse 6000-word list:
- Most passwords: 1-3 attempts
- Occasional: 4-10 attempts
- Rare: 10+ attempts
- Should never hit the 1000 attempt safety limit

## Why This Works

### Length Variability
- Words: 3-15 characters each (varies)
- Numbers: 1-3 digits
- Symbols: 2 characters (always)
- Total range: 6-35+ characters possible

### Target Range: 12-18 characters
With random selection:
- Short words + short number = might be too short → retry
- Long words + long number = might be too long → retry
- Medium combinations = usually just right ✓

### Retry Efficiency
With 6000 words of various lengths, the probability of hitting the 12-18 character sweet spot is high, making retries rare and fast.
