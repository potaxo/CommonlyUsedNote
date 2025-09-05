``` bash
echo 'Disconnected from invalid user Disconnected from 84.211' | sed 's/.*Disconnected from //'
```
``` bash
84.211
```
The `second Disconnected` from is the username.
The regex is **greedy**, it will match as lot as they can.
