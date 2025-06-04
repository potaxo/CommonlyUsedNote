# UsefulVimCommands

## The difference between :q! and :!q :
- :q! is used to quit Vim without saving changes
- :!q try to run an external command.

## Subsitude command:
- :%s/old/new/g replaces all occurrences of 'old' with 'new' in the entire file
- :%s/old/new/gc replaces all occurrences of 'old' with 'new' in the entire file and asks for confirmation before each replacement

### Example:
```vim
:%s/\v<(t|tUp|tRight)\>/&.value/g)>
```
#### Explanation:
- `\v` enables very magic mode, which makes the regex more concise
- `<(t|tUp|tRight)>` matches the words 't', 'tUp', or 'tRight' as whole words
- `&.value` replaces the matched words with themselves followed by '.value'
- `%` applies the substitution to the entire file

### Substitution with visual selection:
1. Select the text you want to replace using visual mode (v or V)
2. Press `:` to enter command mode, which will automatically prepend `:'<,'>` to the command
3. Type `s/old/new/g` to replace 'old' with 'new' in the selected text


## Delete all lines containing a specific word:
- `:g/word/d` deletes all lines containing 'word'
> **Explanation**:  
> `:g` is a global command that applies to all lines in the file    
> `/word/` is a pattern that matches lines containing 'word'    
> `d` is the command to delete the matching lines   