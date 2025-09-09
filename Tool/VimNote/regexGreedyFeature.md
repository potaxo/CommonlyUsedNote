``` bash
echo 'Disconnected from invalid user Disconnected from 84.211' | sed 's/.*Disconnected from //'
```
``` bash
84.211
```
The `second Disconnected` from is the username.
The regex is **greedy**, it will match as lot as they can.

## The truth hehind regex matching
Yes, your thinking is exactly right! You've perfectly described the fundamental behavior of a standard regex engine. It's a systematic, exhaustive search process.

In more formal terms, this process is called **backtracking**. The engine will explore every possible path to find a valid match.

---

### The Search for a Match

Think of the regex engine as someone navigating a maze where each decision is dictated by the regex pattern.

1.  **Follow the Path:** When the engine encounters a greedy quantifier like `.*` or `.+`, it follows the "longest possible match" rule. It rushes ahead as far as it can, consuming characters.
2.  **Hit a Dead End:** If the part of the pattern *after* the greedy quantifier fails to match (like in our previous example), the engine hits a dead end.
3.  **Backtrack and Try Again:** The engine doesn't just give up. It goes back to the last "decision point" (the greedy `.*`) and forces it to give up one character. Then it tries to match the rest of the pattern again from that new position.
4.  **Repeat Until Success or Failure:** It repeats this "advance-fail-backtrack" cycle over and over. If it finds a path where the entire pattern matches, it reports success. If it explores every single possible path in the maze and none of them lead to a full match, it finally reports failure.

Your intuition is correct: for a very complex pattern with many alternations (`|`) and greedy quantifiers (`*`, `+`), the number of possible paths to check can become enormous.

---

### The Hidden Danger: Catastrophic Backtracking ⚠️

This leads to a critical performance issue called **Catastrophic Backtracking**. This happens when a seemingly simple pattern on a specific string creates an exponential number of paths for the engine to check. The result is that the regex match can take seconds, minutes, or even longer, completely freezing your application.

A classic example is a pattern like `(a+)*b`.

Let's say you try to match this against the string `aaaaaaaaaaaaaaaaaaaaaaaaac`.

* The outer `*` tells the `(a+)` group to match as many times as possible.
* The inner `+` tells `a` to match as many times as possible within that group.

The engine will try countless combinations:
* Does `(a+)` match all 25 'a's once? *Fail, no 'b' at the end.*
* Does `(a+)` match 24 'a's, then try to match `(a+)` again with the last 'a'? *Fail, no 'b'.*
* Does `(a+)` match 23 'a's, then try matching `(a+)` on the remaining two 'a's in different ways (`a,a` or `aa`)? *Fail...*

The number of ways to partition the string of 'a's grows exponentially, and the engine has to check **all of them** before it can finally check for the `c` and conclude that the pattern doesn't match.

The fix is usually to make the pattern more specific and avoid this kind of nested, ambiguous repetition. In this case, `a*b` would achieve the same goal without the catastrophic performance hit.

So, you are absolutely right to be thinking about this. The engine's exhaustive search is what makes it powerful, but it's also what requires us to be careful when writing complex patterns!
