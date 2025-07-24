# Basic

### Copy a file to a new place
Great question, potaxo! You're asking:
**"How do I copy a file to another path in Linux?"**
Let’s walk through it step by step.

---

### ✅ **Basic Syntax Using `cp` Command**

To copy a file from one location to another, use the `cp` (copy) command:

```bash
cp source_file destination_path
```

### 📌 **Examples:**

#### 1. **Copy to current directory**

```bash
cp /home/potaxo/file.txt .
```

This copies `file.txt` into your current working directory (`.`).

#### 2. **Copy to another directory**

```bash
cp ~/Documents/file.txt ~/Desktop/
```

This copies `file.txt` from `Documents` to `Desktop`.

#### 3. **Copy and rename at the same time**

```bash
cp ~/Documents/file.txt ~/Desktop/file_copy.txt
```

This also renames it while copying.

#### 4. **With overwrite confirmation (interactive mode)**

```bash
cp -i file.txt ~/backup/
```

It will **ask for confirmation** before overwriting a file in the destination.

#### 5. **Show progress and verbose output**

```bash
cp -v file.txt ~/Desktop/
```

This shows messages like:

```
'file.txt' -> '/home/potaxo/Desktop/file.txt'
```

---

### ⚠️ Notes:

* If the destination is a **directory**, the file keeps its original name.
* If the destination **does not exist**, the file will be **renamed** to that name.
* Use **quotes** if the file path contains **spaces**.

```bash
cp "my file.txt" "/home/potaxo/Backups/"
```

---

### 🧠 Forward-thinking Tip:

Start using `rsync` for more robust copying, especially when:

* Copying large files or folders
* Avoiding redundant copying
* Preserving file permissions and timestamps

Example:

```bash
rsync -avh file.txt ~/Desktop/
```

Let me know if you'd like to learn how to copy entire directories too!
