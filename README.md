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

## 6. MOST COMMON SET OF REFACTORING

### 1. Extract Function
Extract fragment of code into its own function named after its purpose.

```javascript
function printOwing(invoice) {
  printBanner()
  let outstannding = calculateOutstanding()

  // print details
  console.log(`name: ${invoice.customer}`)
  console.log(`amount: ${outstanding}`)
}
```

to

```javascript
function printOwing(invoice) {
  printBanner()
  let outstannding = calculateOutstanding()
  printDetails(outstanding)

  function printDetails(outstanding) {
    console.log(`name: ${invoice.customer}`)
    console.log(`amount: ${outstanding}`)
  }
}
```

**Motivation**:
- You know what's the code doing without reading the details
- Short function is easier to read
- Reduce comment

### 2. Inline Function
Get rid of the function when the body of the code is just as clear as the name

```javascript
function rating(aDriver) {
  return moreThanFiveLateDeliveries(aDriver) ? 2 : 1
}
function moreThanFiveLateDeliveries(aDriver) {
  return aDriver.numberOfLateDeliveries > 5
}
```

to

```javascript
function rating(aDriver) {
  return aDriver.numberOfLateDeliveries > 5 ? 2 : 1
}
```

**Motivation**
- When Indirection is needless (simple delegation) becomes irritating.
- If group of methods are badly factored and grouping them makes it clearer

### 3. Extract Variable
Add a name to an expression

```javascript
//price is base price ­ quantity discount + shipping
return order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100)
```

to

```javascript
const basePrice = order.quantity * order.itemPrice
const quantityDiscount = Math.max(0, order.quantity ­ 500) * order.itemP
const shipping = Math.min(basePrice * 0.1, 100)
return basePrice ­- quantityDiscount + shipping;
```

**Motivation**
- Break down and name a part of a more complex piece of logic
- Easier for debugging

### 4. Inline Variable
Remove variable which doesn't really communicate more than the expression itself.

```javascript
let basePrice = anOrder.basePrice
return (basePrice > 1000)
```

to

```javascript
return anOrder.basePrice > 1000
```

### 5. Change Function Declaration
Rename a function, change list of parameters

```javascript
function circum(radius) {...}
```

to

```javascript
function circumference(radius) {...}
```

**Motivation**
- Easier to understand
- Easier to reuse, sometime better encapsulation

### 6. Encapsulate Variable
Encapsulate a reference to some data structure

```javascript
let defaultOwner = {firstName: "Martin", lastName: "Fowler"}
```

to

```javascript
// defaultOwner.js
let defaultOwnerData = {firstName: "Martin", lastName: "Fowler"}
export function defaultOwner() { return defaultOwnerData }
export function setDefaultOwner(arg) { defaultOwnerData = arg }
```

**Motivation**
- Provide a clear point to monitor changes and use of the data, like validation.

### 7. Rename Variable
Make shared variable's name can self-explain

```javascript
let a = height * width
```

to

```javascript
let area = height * width
```

### 8. Introduce Parameter Object
Replace groups of data items that regularly travel together with a single data structure

```javascript
function amountInvoiced(startDate, endDate) {}
function amountReceived(startDate, endDate) {}
function amountOverdue(startDate, endDate) {}
```

to

```javascript
function amountInvoiced(aDateRange) {}
function amountReceived(aDateRange) {}
function amountOverdue(aDateRange) {}
```

**Motivation**
- Make explicit the relationship between the data items
- Reduce the size of parameter list
- Make code more consistent
- Enable deeper changes to the code

### 9. Combine Functions Into Class
Form a class base on group of functions that operate closely on a common data

```javascript
function base(aReading) {}
function taxableCharge(aReading) {}
function calculateBaseCharge(aReading) {}
```

to

```javascript
class Reading() {
  base() {}
  taxableCharge() {}
  calculateBaseCharge() {}
}
```

**Motivation**
- Simplify function call by removing many arguments
- Easier to pass object to other parts of the system

### 10. Combine Functions Into Transform
Takes the source data as input and calculates all the derivations, putting each derived value as a field in the output data

```javascript
function base(aReading) {}
function taxableCharge(aReading) {}
```

to

```javascript
function enrichReading(argReading) {
  const aReading = _.cloneDeep(argReading)
  aReading.baseCharge = base(aReading)
  aReading.taxableCharge = taxableCharge(aReading)
  return aReading
}
```

**Motivation**
- Avoid duplication of logic

### 11. Split Phase
Split code which do different things into separate modules

```javascript
const orderData = orderString.split(/\s+/)
const productPrice = priceList[orderData[0].split("-")[1]]
const orderPrice = parseInt(orderData[1]) * productPrice
```

to

```javascript
const orderRecord = parseOrder(orderString)
const orderPrice = price(orderRecord, priceList)

function parseOrder(aString) {
  const values = aString.split(/\s+/)
  return {
    productID: values[0].split("-")[1],
    quantity: parseInt(values[1])
  }
}
function price(order, priceList) {
  return order.quantity * priceList[order.productID]
}
```

**Motivation**
- Make the different explicit, revealing the different in the code
- Be able to deal with each module separately

## 7. ENCAPSULATION

### 1. Encapsulate Record
Create record (class) from object

```javascript
organization = {name: "Acme gooseberries", country: "GB"}
```

to

```javascript
class Organization {
  constructor(data) {
    this._name = data.name
    this._country = data.country
  }

  get name() { return this._name }
  set name(arg) { this._name = arg }
  get country() { retrun this._country }
  set country(arg) { this._country = arg }
}
```

**Motivation**
- Hide what's stored and provide methods to get value
- Easier to refactoring, for example: rename

### 2. Encapsulate Collection
A method returns a collection. Make it return a read-only view and provide add/remove methods

```javascript
class Person {
  get courses() { return this._courses }
  set courses(aList) { this._courses = aList }
}
```

to

```javascript
class Person {
  get courses() { return this._courses.slice() }
  addCourse(aCourse) {}
  removeCourse(aCourse)
}
```

**Motivation**
- Change to the collection should go through the owning class to prevent unexpected changes.
- Prevent modification of the underlying collection for example: return a copy or read-only proxy instead of collection value

### 3. Replace Primitive with Object
Create class for data

```javascript
orders.filter(o => "high" === o.priority || "rush" === o.priority)
```

to

```javascript
orders.filter(o => o.priority.higherThan(new Priority("normal")))
```

**Motivation**
- Encapsulate behaviour with data

### 4. Replace Temp with Query
Extract the assignment of the variable into a function

```javascript
const basePrice = this._quantity * this._itemPrice
if (basePrice > 1000) {
  return basePrice * 0.95
} else {
  return basePrice * 0.98
}
```

to

```javascript
get basePrice() { return this._quantity * this._itemPrice }
//...
if (this.basePrice > 1000) {
  return this.basePrice * 0.95
} else {
  return this.basePrice * 0.98
}
```

**Motivation**
- Avoid duplicating the calculation logic in similar functions

### 5. Extract Class
Extract class base on a subset of data and a subset of methods

```javascript
class Person {
  get officeAreaCode() { return this._officeAreaCode }
  get officeNumber() { return this._officeNumber }
}
```

to

```javascript
class Person {
  get officeAreaCode() { return this._telephoneNumber.areaCode }
  get officeNumber() { return this._telephoneNumber.number }
}
class TelephoneNumber {
  get areaCode() { return this._areaCode }
  get number() { return this._number }
}
```

**Motivation**
- Smaller class is easier to understand
- Separate class's responsibility

### 6. Inline Class
Merge class if class isn't doing very much. Move its feature to another class then delete it.

```javascript
class Person {
  get officeAreaCode() { return this._telephoneNumber.areaCode }
  get officeNumber() { return this._telephoneNumber.number }
}
class TelephoneNumber {
  get areaCode() { return this._areaCode }
  get number() { return this._number }
}
```

to

```javascript
class Person {
  get officeAreaCode() { return this._officeAreaCode }
  get officeNumber() { return this._officeNumber }
}
```

**Motivation**
- Class is no longer pulling its weight and shouldn’t be around any more
- When want to refactor pair of classes. First Inline Class -> Extract Class to make new separation

### 7. Hide Delegate
A client is calling a delegate class of an object, create methods on the server to hide the delegate.

```javascript
manager = aPerson.department.manager
```

to

```javascript
manager = aPerson.manager

class Person {
  get manager() {
    return this.department.manager
  }
}
```

**Motivation**
- Client doesn't need to know and response to delegation's change
- Better encapsulation

### 8. Remove Middle Man
Client call the delegate directly

```javacript
manager = aPerson.manager

class Person {
  get manager() {
    return this.department.manager
  }
}
```

to

```javascript
manager = aPerson.department.manager
```

**Motivation**
- When there are too many delegating methods

### 9. Substitute Algorithm
Replace complicated algorithm with simpler algorithm

```javascript
function foundPerson(people) {
  for (let i = 0; i < people.lenth; i++) {
    if (people[i] === "Don") {
      return "Don"
    }
    if (people[i] === "John") {
      return "John"
    }
    if (people[i] === "Kent") {
      return "Kent"
    }
  }
  return ""
}
```

to

```javascript
function foundPerson(people) {
  const candidates = ["Don", "John", "Kent"]
  return people.find(p => candidates.includes(p)) || ""
}
```

**Motivation**
- Change to algorithm which make changes easier
- The clearer algorithm is, the better.

## 8. MOVING FEATURES

### 1. Move Function
Move a function when it references elements in other contexts more than the one it currently resides in

```javascript
class Account {
  get overdraftCharge() {}
}
class AccountType {}
```

to

```javascript
class Account {}
class AccountType {
  get overdraftCharge() {}
}
```

**Motivation**
- Improve encapsulation, loose coupling

### 2. Move Field
Move field from once class to another

```javascript
class Customer {
  get plan() { return this._plan }
  get discountRate() { return this._discountRate }
}
```

to

```javascript
class Customer {
  get plan() { return this._plan }
  get discountRate() { return this.plan.discountRate }
}
```

**Motivation**
- Pieces of data that are always passed to functions together are usually best put in a single record
- If a change in one record causes a field in another record to change too, that’s a sign of a field in the wrong place

### 3. Move Statements into Function
When statement is a part of called functions (always go togeter), move it inside the function

```javascript
result.push(`<p>title: ${person.photo.title}</p>`)
result.concat(photoData(person.photo))

function photoData(aPhoto) {
  return [
    `<p>location: ${aPhoto.location}</p>`,
    `<p>date: ${aPhoto.data.toDateString()}</p>`
  ]
}
```

to

```javascript
result.concat(photoData(person.photo))

function photoData(aPhoto) {
  return [
    `<p>title: ${aPhoto.title}</p>`,
    `<p>location: ${aPhoto.location}</p>`,
    `<p>date: ${aPhoto.data.toDateString()}</p>`
  ]
}
```

**Motivation**
- Remove duplicated code

### 4. Move Statements To Callers

```javascript
emitPhotoData(outStream, person.photo)

function emitPhotoData(outStream, photo) {
  outStream.write(`<p>title: ${photo.title}</p>\n`)
  outStream.write(`<p>location: ${photo.location}</p>\n`)
}
```

to

```javascript
emitPhotoData(outStream, person.photo)
outStream.write(`<p>location: ${photo.location}</p>\n`)

function emitPhotoData(outStream, photo) {
  outStream.write(`<p>title: ${photo.title}</p>\n`)
}
```

**Motivation**
- When common behavior used in several places needs to vary in some of its call

### 5. Replace Inline Code with Function Call
Replace the inline code with a call to the existing function

```javascript
let appliesToMass = false
for (const s of states) {
  if (s === "MA") appliesToMass = true
}
```

```javascript
appliesToMass = states.includes("MA")
```

**Motivation**
- Remove duplication
- Meaningful function name is easier to understand

### 6. Slide Statements
Move related code to near each other

```javascript
const pricingPlan = retrievePricingPlan()
const order = retreiveOrder()
let charge
const chargePerUnit = pricingPlan.unit
```

to

```javascript
const pricingPlan = retrievePricingPlan()
const chargePerUnit = pricingPlan.unit
const order = retreiveOrder()
let charge
```

**Motivation**
- It makes code easier to understand and easier to extract function

### 7. Split Loop
Split the loop which does two different things

```javascript
let averageAge = 0
let totalSalary = 0
for (const p of people) {
  averageAge += p.age
  totalSalary += p.salary
}
averageAage = averageAge / people.length
```

to

```javascript
let totalSalary = 0
for (const p of people) {
  totalSalary += p.salary
}

let averageAge = 0
for (const p of people) {
  averageAge += p.age
}
averageAge = averageAge / people.length
```

**Motivation**
- Easier to use
- Easier to understand because each loop will do only 1 thing

### 8. Replace Loop with Pipeline
Replace loop with collection pipeline, like `map` or `filter`

```javascript
const names = []
for (const i of input) {
  if (i.job === "programmer") {
    names.push(i.name)
  }
}
```

to

```javascript
const names = input.filter(i => i.job === "programmer").
                    map(i => i.name)
```

**Motivation**
- Easier to understand the flow of data

### 9. Remove Dead Code

```javascript
if (false) {
  doSomethingThatUsedToMatter()
}
```

to

```javascript

```

**Motivation**
- Easier and quicker for developer to understand the codebase


## 9. ORGANIZING DATA

### 1. Split Variable
Any variable with more than one responsibility should be replaced with multiple variables, one for each responsibility

```javascript
let temp = 2 * (height + width)
console.log(temp)
temp = height * width
console.log(temp)
```

to

```javascript
const perimeter = 2 * (height + width)
console.log(perimeter)
const area = height * width
console.log(area)
```

**Motivation**
- Easier to understand

### 2. Rename Field

```javascript
class Organization {
  get name() {}
}
```

to

```javascript
class Organization {
  get title() {}
}
```

### 3. Replace Derived Variable With Query
Remove anny variables which cloud be easily calculate

```javascript
get discountedTotal() { return this._discountedTotal }
set discount(aNumber) {
  const old = this._discount
  this._discount = aNumber
  this._discountedTotal += old - aNumber
}
```

to

```javascript
get discountedTotal() { return this._baseTotal - this._discount }
set discount() { this._discount = aNumber }
```

**Motivation**
- Minimize scope of mutable data
- A calculate makes it clearer what the meaning of data is

### 4. Change Reference To Value
Treat data as value. When update, replace entire inner object with a new one

```javascript
class Product {
  applyDiscount(arg) {
    this._price.amount -= arg
  }
}
```

to

```javascript
class Product {
  applyDiscount(arg) {
    this._price = new Money(this._price.amount - arg, this._price.currency)
  }
}
```

**Motivation**
- Immutable data is easier to deal with

### 5. Change Value To Reference
When need to share an object in different place, or have duplicated objects

```javascript
let customer = new Customer(customerData)
```

to

```javascript
let customer = customerRepository.get(customerData.id)
```

**Motivation**
- Update one reference is easier and more consistent than update multiple copies

## 10. SIMPLIFYING CONDITIONAL LOGIC

## 11. REFACTORING APIS

## 12. DEALING WITH INHERITANCE