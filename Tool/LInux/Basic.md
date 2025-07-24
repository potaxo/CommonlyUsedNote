# Basic


---
### Copy a file to a new place
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
### use grep to search
```
conda list | grep <package-name>
```

