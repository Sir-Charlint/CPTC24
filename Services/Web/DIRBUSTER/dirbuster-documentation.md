Here's a  DirBuster cheat sheet, specifically for use cptc

### **DirBuster Command-Line Cheat Sheet (Kali Linux)**

DirBuster can be run in command-line mode, making it faster and more efficient for headless or automated tasks.

#### 1. **Basic Command-Line Usage**
```bash
dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50
```

#### 2. **Options Explained**
   - `-u` : The target URL. Example: `http://example.com`
   - `-l` : Path to the wordlist.
     - Common wordlist options on Kali:
       - `/usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt` (standard wordlist)
       - `/usr/share/dirbuster/wordlists/directory-list-2.3-small.txt` (smaller, faster)
       - `/usr/share/dirbuster/wordlists/directory-list-lowercase-2.3-medium.txt` (lowercase only)
   - `-t` : Number of threads. Adjust based on your system and target. Example: `-t 50`

#### 3. **File Extensions**
   Use the `-e` flag to specify file extensions to search for:
```bash
dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -e php,html,txt
```
   - **Example Extensions:** `php, html, js, css, txt`

#### 4. **Recursive Search**
   By default, DirBuster in CLI mode will search directories recursively.
   - You can use `-r` to control recursion depth:
```bash
dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -e php,html -r 3
```
   - `-r 3` means search 3 levels deep in directories.

#### 5. **Timeouts and Delays**
   Use the `-o` flag to set timeouts and delays between requests (in milliseconds), which is helpful to avoid overwhelming the target server:
```bash
dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -o 500
```
   - Example: `-o 500` introduces a 500ms delay between requests.

#### 6. **Save Results**
   - Save the scan output to a file for later analysis using `-O`:
```bash
dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -e php,html,txt -O results.txt
```

#### 7. **Use a Proxy**
   - If you want to run DirBuster through a proxy, use the `-p` option:
```bash
dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -e php,html,txt -p http://localhost:8080
```
   - This is useful if you're using **Burp Suite** or another proxy to monitor traffic.

#### 8. **Help Menu**
   - To see all available options and flags:
```bash
dirbuster --help
```

---

### **Example Scenarios**

1. **Quick Scan Default Settings**:
   ```bash
   dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt -t 20
   ```

2. **Brute Forc Multiple Extensions**:
   ```bash
   dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -e php,html,txt,js
   ```

3. **Recursive Scan with Custom Depth and Saving Results**:
   ```bash
   dirbuster -u http://example.com -l /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -t 50 -r 5 -O results.txt
   ```
understanding common results

404: page does not exist in its entirety
403: restricted access, possible admin page or  private-info page, good to right down
200: accessible, check the html for possible vulns
