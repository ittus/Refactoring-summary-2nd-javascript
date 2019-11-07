## 3. BAD SMELLS IN CODE

### 1. Mysterious name
Name should clearly communicate what they do and how to use them

### 2. Duplicated code
Same code structure in more than one place

### 3. Long function
the longer a function is, the more difficult it is to understand

### 4. Long parameter list
Difficult to understand and easily introduce bug

### 5. Global data
Global variable is difficult to track and debug

### 6. Mutable data
Changes to data can often lead to unexpected consequences and tricky bugs

### 7. Divergent change
One module is changed in different ways for different reasons

### 8. Shotgun surgery
When every time you make a change, you have to make a lot of little edits to a lot of different classes

### 9. Feature envy
When a function in one module spends more time communicating with functions or data inside another module than it does within its own module

### 10. Data clumps
Same three or four data items together in lots of places

### 11. Primittive Obsession
Use primitive types instead of custom fundamental types

### 12. Repeated switches
Same conditional switching logic pops up in different places

### 13. Loops
Using loops instead of first-class functions such as filter or map

### 14. Lazy element
A class, struct or function that isn't doing enough to pay for itself should be eliminated.

### 15. Speculative generality
All sorts of hooks and special cases to handle things that aren’t required

### 16. Temporary field
An instance variable that is set only in certain circumstances.

### 17. Message chains
Ưhen a client asks one object for another object, which the client then asks for yet another object...

### 18. Middle man
When an object delegates much of its functionality.

### 19. Insider trading
Modules that whisper to each other by the coffee machine need to be separated by using Move Function and Move Field to reduce the need to chat.

### 20. Large class
A class is trying to do too much, it often shows up as too many fields

### 21. Alternative Classes with Different Interfaces
Classes with methods that look to similar.

### 22. Data class
Classes that have fields, getting and setting methods for the fields, and nothing else

### 23. Refused Bequest
Subclasses doesn't make uses of parents method

### 24. Comment
The comments are there because the code is bad