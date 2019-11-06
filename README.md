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