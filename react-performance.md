# Keys when mapping lists

To increase performance, you shouldn't provide the index from a .map to an element because by providing a UNIQUE key will tell react that nothing has changed here and it can keep skipping over the list until it finds a newly added element. 

## Example

```
<ul>
  <li>Phantom Menace</li>
  <li>Attack of the Clones</li>
</ul>

// As you can see in the new list that the "Phantom Menace" & "Attack of the Clones" have shifted down by one, which means the whole list is now being needed to be compared by React, because they have different places in the list now.

<ul>
  <li>Revenge of the Sith</li>
  <li>Phantom Menace</li>
  <li>Attack of the Clones</li>
</ul>
```

# Learnt from
Watch this video from 10:30 to the end:
https://www.youtube.com/watch?v=za2FZ8QCE18
