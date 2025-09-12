Here’s a concise summary of the difference:

* **With `[ ... ]` (test command):**
  The shell does **string, number, or file tests**, not command execution.
  Example:

  ```sh
  if [ -f myfile.txt ]; then
      echo "File exists"
  fi
  ```

  Here `[ -f myfile.txt ]` checks if the file exists.

* **Without `[ ]` (bare command):**
  The shell uses the **exit status of the command itself** as the condition.
  Example:

  ```sh
  if grep -q "hello" file.txt; then
      echo "Found"
  fi
  ```

  Here `grep` runs, and its success/failure decides the branch.

---

**Key difference:**

* `[ ]` → evaluate an expression (strings, numbers, file attributes).
* `command` → run the command and use its exit code (`0 = success`, non-zero = failure).

Forward-thinking tip: combine them wisely. For example, `if [ "$(command)" = "something" ]; then ...` runs a command but still uses `[ ]` to test its *output*. This mixing is very common in real scripts.
