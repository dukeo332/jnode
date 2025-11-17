# Deployment Checklist

## Phase 1: Initial Setup ✓

- [x] Project structure created
- [x] All source files in place
- [x] Placeholder wordlist included (100 words)
- [x] Network configuration set for mint2025
- [x] Documentation complete

## Phase 2: Upload to Server

- [ ] Upload entire `password-generator` folder to `/home/mpstech/mynodes/`
- [ ] SSH into mint2025
- [ ] Navigate to project: `cd ~/mynodes/password-generator`
- [ ] Verify all files present: `ls -la`

## Phase 3: Install Dependencies

- [ ] Run: `npm install`
- [ ] Wait for installation to complete
- [ ] Verify no errors in output

## Phase 4: Test with Placeholder

- [ ] Start dev server: `npm run dev`
- [ ] Note the port (likely 5173)
- [ ] Open browser to: `http://mint2025:5173`
- [ ] Verify app loads correctly

### Functionality Tests
- [ ] Click "Generate Password" - creates a password
- [ ] Password displays in the box
- [ ] Password is 12-18 characters long
- [ ] Click "Copy" - button changes to "Copied!"
- [ ] Paste somewhere to verify clipboard works
- [ ] Generation Details section appears
- [ ] Statistics section appears and updates

### Detail Verification
- [ ] "Words" shows the 2 words used
- [ ] "Number" shows the digit(s) used
- [ ] "Symbols" shows the 2 symbols used
- [ ] "Pattern" shows the assembly order
- [ ] "Length" matches actual password length

### Generate Multiple Passwords
- [ ] Click generate 10 times
- [ ] All passwords are different
- [ ] All passwords are 12-18 characters
- [ ] Statistics update correctly
- [ ] Attempt counts are reasonable (1-10)

## Phase 5: Prepare Full Wordlist

### Your Wordlist File
- [ ] Have your 6000-word text file ready
- [ ] One word per line
- [ ] No extra blank lines at start/end
- [ ] Encoding is UTF-8

### Conversion Method Choice

**Option A: Use Conversion Script**
- [ ] Copy wordlist to project directory
- [ ] Run: `node convert-wordlist.js your-file.txt > src/wordlist.js`
- [ ] Verify output looks correct
- [ ] Check word count in terminal output

**Option B: Manual Conversion**
- [ ] Open your wordlist in text editor
- [ ] Format as JavaScript array
- [ ] Save as `src/wordlist.js`
- [ ] Verify format matches placeholder structure

## Phase 6: Deploy Full Wordlist

- [ ] Back up placeholder: `cp src/wordlist.js src/wordlist.backup.js`
- [ ] Replace with full list (using chosen method above)
- [ ] Dev server auto-reloads (if running)
- [ ] Or restart: `npm run dev`

### Test with Full List
- [ ] App loads (may take a moment first time)
- [ ] Generate password successfully
- [ ] Generate 20+ passwords to test variety
- [ ] Verify different words appearing
- [ ] Check attempt statistics (should still be low)
- [ ] No errors in browser console (F12)

## Phase 7: Final Verification

### Browser Testing
- [ ] Test on Chrome/Edge
- [ ] Test on Firefox
- [ ] Test on Safari (if available)
- [ ] Test on mobile device

### Network Access
- [ ] Access from your workstation
- [ ] Access from another device on network
- [ ] Verify hostname works: `http://mint2025:5173`
- [ ] Verify IP works: `http://10.152.1.72:5173`

### Performance Check
- [ ] Password generation is instant
- [ ] Copy function works reliably
- [ ] No lag or delays
- [ ] Page responsive on all devices

## Phase 8: Production Build (Optional)

If you want to build for Apache deployment:

- [ ] Run: `npm run build`
- [ ] Check `dist/` folder created
- [ ] Test preview: `npm run preview`
- [ ] Copy `dist/` contents to `/var/www/html/password-gen/`
- [ ] Test at: `http://mint2025/password-gen/`

## Troubleshooting Guide

### Issue: npm install fails
**Solution:**
- Check Node.js version: `node --version`
- Should be 20.x
- Update if needed: `curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -`

### Issue: Can't access from network
**Solution:**
- Verify vite.config.js has correct settings
- Check firewall: `sudo ufw status`
- Try IP instead of hostname
- Check dev server is running

### Issue: App loads but won't generate passwords
**Solution:**
- Check browser console (F12) for errors
- Verify wordlist.js format is correct
- Check export statement is present
- Restart dev server

### Issue: Passwords too long/short
**Solution:**
- This shouldn't happen with retry loop
- Check wordlist for extremely long words (15+ characters)
- Verify retry logic in App.vue

### Issue: Copy button doesn't work
**Solution:**
- Clipboard API requires HTTPS or localhost
- For internal HTTP, some browsers may block
- Test in different browser
- Check browser console for security errors

## Success Criteria

✓ App loads on network  
✓ Generates valid passwords (12-18 chars)  
✓ Copy function works  
✓ Details display correctly  
✓ Statistics track properly  
✓ Works with full 6000-word list  
✓ Responsive on mobile  
✓ No console errors  
✓ Fast performance  

## Post-Deployment

- [ ] Document the URL for your team
- [ ] Consider bookmarking for easy access
- [ ] Monitor for any issues
- [ ] Collect feedback from users

## Future Enhancements (Ideas)

- [ ] Add password strength indicator
- [ ] Allow custom length ranges
- [ ] Option to exclude certain symbols
- [ ] Password history (session-based)
- [ ] Export passwords to file
- [ ] Dark mode toggle
- [ ] Remember user preferences

---

**Current Status:** ⬜ Not Started | 🟨 In Progress | ✅ Complete

Mark items as you complete them!
