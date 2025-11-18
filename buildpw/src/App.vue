<template>
  <div class="app-container">
    <div class="password-card">
      <h1>Password Generator</h1>
      
      <div class="controls">
        <label for="quantity">Number of passwords:</label>
        <input 
          id="quantity" 
          type="number" 
          v-model.number="quantity" 
          min="1" 
          max="100"
          class="quantity-input"
        />
        <div class="difficulty-toggle">
          <label>Difficulty:</label>
          <button 
            @click="difficulty = 'hard'" 
            :class="{ active: difficulty === 'hard' }"
            class="toggle-btn"
          >
            Hard
          </button>
          <button 
            @click="difficulty = 'easy'" 
            :class="{ active: difficulty === 'easy' }"
            class="toggle-btn"
          >
            Easy
          </button>
        </div>
      </div>
      
      <button @click="generatePasswords" class="generate-btn">
        Generate Passwords
      </button>
      
      <div v-if="passwords.length > 0" class="password-list-container">
        <div class="list-header">
          <span>{{ passwords.length }} password{{ passwords.length > 1 ? 's' : '' }} generated</span>
          <button @click="copyAllPasswords" class="copy-all-btn">
            {{ copyButtonText }}
          </button>
        </div>
        <textarea 
          ref="passwordTextarea"
          class="password-list" 
          :value="passwords.join('\n')" 
          readonly
          rows="15"
        ></textarea>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { words } from './wordlist.js'
import { myWords } from './easywords.js'

const passwords = ref([])
console.log('wordlist length:', words.length)
const quantity = ref(10)
const difficulty = ref('hard') // 'hard' or 'easy'
const copyButtonText = ref('Copy All')
const passwordTextarea = ref(null)

// Symbol arrays
const symbolsNonShift = ['`', '-', '=', '[', ']', '\\', ';', "'", ',', '.', '/']
const symbolsShift = ['~', '!', '@', '#', '$', '%', '^', '&', '*', '(', ')', '_', '+', '{', '}', '|', ':', '"', '<', '>', '?']

// Get random element from array
const getRandomElement = (arr) => arr[Math.floor(Math.random() * arr.length)]

// Generate a random number between min and max (inclusive)
const getRandomNumber = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min

const generateEasyPassword = () => {
  const MIN_LENGTH = 8
  const MAX_LENGTH = 12
  let attempts = 0
  
  // Randomly choose between 2 words or 1 word
  const useTwoWords = Math.random() < 0.5
  
  while (true) {
    attempts++
    
    // Pick a number between 0-20
    const number = getRandomNumber(0, 20)
    
    // Pick a symbol (= or -)
    const symbol = Math.random() < 0.5 ? '=' : '-'
    
    let assembledPassword
    
    if (useTwoWords) {
      // Mode 1: 2 short words + number
      const word1 = getRandomElement(myWords).toLowerCase()
      const word2 = getRandomElement(myWords).toLowerCase()
      
      // Random arrangement
      const arrangement = getRandomNumber(0, 2)
      if (arrangement === 0) {
        assembledPassword = `${word1}${symbol}${word2}${symbol}${number}`
      } else if (arrangement === 1) {
        assembledPassword = `${word1}${symbol}${number}${symbol}${word2}`
      } else {
        assembledPassword = `${number}${symbol}${word1}${symbol}${word2}`
      }
    } else {
      // Mode 2: 1 longer word + number
      const word = getRandomElement(myWords).toLowerCase()
      
      // Random arrangement
      assembledPassword = Math.random() < 0.5 
        ? `${word}${symbol}${number}`
        : `${number}${symbol}${word}`
    }
    
    // Check if password length is within range
    if (assembledPassword.length >= MIN_LENGTH && assembledPassword.length <= MAX_LENGTH) {
      return assembledPassword
    }
    
    // Safety check: prevent infinite loops
    if (attempts > 1000) {
      console.error('Could not generate valid easy password after 1000 attempts')
      return null
    }
  }
}

const generateSinglePassword = () => {
  const MIN_LENGTH = 12
  const MAX_LENGTH = 18
  let attempts = 0
  
  // Keep trying until we get a password within the desired length range
  while (true) {
    attempts++
    
    // Step 1: Pick 2 random words
    const word1 = getRandomElement(words)
    const word2 = getRandomElement(words)
    
    // Step 2: Generate random number (1-3 digits)
    const numberDigits = getRandomNumber(1, 3)
    const minNum = numberDigits === 1 ? 0 : Math.pow(10, numberDigits - 1)
    const maxNum = Math.pow(10, numberDigits) - 1
    const number = getRandomNumber(minNum, maxNum)
    
    // Step 3: Determine number placement (0 = beginning, 1 = middle, 2 = end)
    const numberPlacement = getRandomNumber(0, 2)
    
    // Step 4: Pick ONE symbol to use for both separators
    const symbolArray = Math.random() < 0.5 ? symbolsNonShift : symbolsShift
    const symbol = getRandomElement(symbolArray)
    const isShiftedSymbol = symbolsShift.includes(symbol)
    
    // Step 5: Apply capitalization based on number placement and symbol type
    let finalWord1, finalWord2
    
    if (numberPlacement === 1) {
      // Number in middle: randomly uppercase EITHER first OR second word
      const uppercaseFirst = Math.random() < 0.5
      finalWord1 = uppercaseFirst ? word1.toUpperCase() : word1
      finalWord2 = uppercaseFirst ? word2 : word2.toUpperCase()
    } else {
      // Number NOT in middle (word is in middle position)
      if (isShiftedSymbol) {
        // Shifted symbol: UPPERCASE the middle word
        if (numberPlacement === 0) {
          // Pattern: number-WORD-word
          finalWord1 = word1.toUpperCase()
          finalWord2 = word2
        } else {
          // Pattern: word-WORD-number
          finalWord1 = word1
          finalWord2 = word2.toUpperCase()
        }
      } else {
        // Non-shifted symbol: UPPERCASE one of the non-middle words
        if (numberPlacement === 0) {
          // Pattern: number-word-WORD
          finalWord1 = word1
          finalWord2 = word2.toUpperCase()
        } else {
          // Pattern: WORD-word-number
          finalWord1 = word1.toUpperCase()
          finalWord2 = word2
        }
      }
    }
    
    // Step 6: Assemble password based on number placement (using same symbol twice)
    let assembledPassword
    if (numberPlacement === 0) {
      assembledPassword = `${number}${symbol}${finalWord1}${symbol}${finalWord2}`
    } else if (numberPlacement === 1) {
      assembledPassword = `${finalWord1}${symbol}${number}${symbol}${finalWord2}`
    } else {
      assembledPassword = `${finalWord1}${symbol}${finalWord2}${symbol}${number}`
    }
    
    // Check if password length is within range
    if (assembledPassword.length >= MIN_LENGTH && assembledPassword.length <= MAX_LENGTH) {
      return assembledPassword
    }
    
    // Safety check: prevent infinite loops (shouldn't happen with our wordlist)
    if (attempts > 1000) {
      console.error('Could not generate valid password after 1000 attempts')
      return null
    }
  }
}

const generatePasswords = () => {
  const newPasswords = []
  const count = Math.min(Math.max(1, quantity.value), 100) // Clamp between 1-100
  
  for (let i = 0; i < count; i++) {
    const password = difficulty.value === 'easy' 
      ? generateEasyPassword() 
      : generateSinglePassword()
    if (password) {
      newPasswords.push(password)
    }
  }
  
  passwords.value = newPasswords
  copyButtonText.value = 'Copy All'
}

const copyAllPasswords = async () => {
  if (passwords.value.length === 0) return

  const text = passwords.value.join('\n')

  // Try modern Clipboard API first (requires secure context / localhost)
  try {
    if (navigator.clipboard && navigator.clipboard.writeText) {
      await navigator.clipboard.writeText(text)
    } else {
      throw new Error('Clipboard API not available')
    }

    copyButtonText.value = 'Copied!'
    setTimeout(() => {
      copyButtonText.value = 'Copy All'
    }, 2000)
    return
  } catch (err) {
    // fallback below
    console.warn('Clipboard API failed, falling back to execCommand copy:', err)
  }

  // Fallback for insecure origins / older browsers: use a temporary textarea + execCommand('copy')
  try {
    const ta = document.createElement('textarea')
    ta.value = text
    // keep it out of view
    ta.style.position = 'fixed'
    ta.style.left = '-9999px'
    document.body.appendChild(ta)
    ta.select()
    ta.setSelectionRange(0, ta.value.length)
    const ok = document.execCommand('copy')
    document.body.removeChild(ta)

    if (ok) {
      copyButtonText.value = 'Copied!'
      setTimeout(() => {
        copyButtonText.value = 'Copy All'
      }, 2000)
    } else {
      throw new Error('execCommand copy returned false')
    }
  } catch (err2) {
    console.error('Failed to copy:', err2)
    copyButtonText.value = 'Failed'
    setTimeout(() => {
      copyButtonText.value = 'Copy All'
    }, 2000)
  }
}

// Generate initial passwords on mount
generatePasswords()
</script>

<style scoped>
.app-container {
  min-height: 100vh;
  /* updated: medium -> dark blue gradient */
  background: linear-gradient(135deg, #2B6CB0 0%, #1E3A8A 100%);
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
}

.password-card {
  background: white;
  border-radius: 1rem;
  padding: 2rem;
  max-width: 700px;
  width: 100%;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
}

h1 {
  margin: 0 0 1.5rem 0;
  color: #333;
  text-align: center;
  font-size: 2rem;
}

.controls {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 1rem;
}

.controls label {
  font-weight: 600;
  color: #555;
}

.quantity-input {
  flex: 1;
  padding: 0.75rem;
  border: 2px solid #2B6CB0;
  border-radius: 0.5rem;
  font-size: 1rem;
  font-family: 'Courier New', monospace;
  max-width: 100px;
}

.quantity-input:focus {
  outline: none;
  /* slightly darker blue on focus */
  border-color: #1B4F8B;
  box-shadow: 0 0 0 3px rgba(43, 108, 176, 0.1);
}

.difficulty-toggle {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.difficulty-toggle label {
  margin: 0;
}

.toggle-btn {
  padding: 0.5rem 1rem;
  border: 2px solid #2B6CB0;
  background: white;
  color: #2B6CB0;
  border-radius: 0.5rem;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  transition: all 0.3s ease;
}

.toggle-btn:hover {
  background: #f0f0f0;
}

.toggle-btn.active {
  background: #2B6CB0;
  color: white;
}

.generate-btn {
  width: 100%;
  background: linear-gradient(135deg, #2B6CB0 0%, #1E3A8A 100%);
  color: white;
  border: none;
  padding: 1rem;
  border-radius: 0.5rem;
  cursor: pointer;
  font-size: 1.1rem;
  font-weight: 600;
  transition: all 0.3s ease;
  margin-bottom: 1.5rem;
}

.generate-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(43, 108, 176, 0.4);
}

.password-list-container {
  background: #f9f9f9;
  border-radius: 0.5rem;
  padding: 1.5rem;
}

.list-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  padding-bottom: 0.5rem;
  border-bottom: 2px solid #2B6CB0;
}

.list-header span {
  font-weight: 600;
  color: #555;
}

.copy-all-btn {
  background: #2B6CB0;
  color: white;
  border: none;
  padding: 0.5rem 1.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 600;
  transition: all 0.3s ease;
}

.copy-all-btn:hover {
  background: #1B4F8B;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(43, 108, 176, 0.4);
}

.password-list {
  width: 100%;
  padding: 1rem;
  border: 2px solid #2B6CB0;
  border-radius: 0.5rem;
  font-family: 'Courier New', monospace;
  font-size: 1rem;
  line-height: 1.6;
  resize: vertical;
  background: white;
  color: #333;
}

.password-list:focus {
  outline: none;
  border-color: #1B4F8B;
  box-shadow: 0 0 0 3px rgba(43, 108, 176, 0.1);
}

@media (max-width: 640px) {
  .password-card {
    padding: 1.5rem;
  }
  
  h1 {
    font-size: 1.5rem;
  }
  
  .controls {
    flex-direction: column;
    align-items: stretch;
  }
  
  .quantity-input {
    max-width: none;
  }
  
  .difficulty-toggle {
    width: 100%;
    justify-content: space-between;
  }
  
  .toggle-btn {
    flex: 1;
  }
  
  .list-header {
    flex-direction: column;
    gap: 0.5rem;
    align-items: stretch;
  }
  
  .copy-all-btn {
    width: 100%;
  }
}
</style>
