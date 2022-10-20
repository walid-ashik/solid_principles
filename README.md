# Why S.O.L.I.D ?

The broad goal of the SOLID principles is to **reduce dependencies so that engineers change one area of software without
impacting others.** Additionally, they’re intended to make designs easier to understand, maintain, and extend.

Using these design principles makes it easier for software engineers to avoid issues and to build **adaptive, effective,
and agile software.**

# Problems & Solution using SOLID

**Readable:** We are constantly reading old code as part of the
effort to write code. We spend more time reading
than writing code. This becomes more complex day by day when
more code is added & team grows.

**Testable:** It should be easy to write automated tests that
assert the system’s behavior. The profound reason is that we
don’t want to spend too much time verifying that the system is
effective. Running manual tests are very costlier than building software.

**Extensible:** It should be easy to add additional functionality
to the system. A big part of this comes
from readability. Readability is a necessary but not a sufficient
element to enable extensibility. We implement SOLID patterns which enable readability.

**Robust:** Adding additional functionality should not
introduce much risk of breaking existing functionality.

**Maintainable:** When a defect is reported, it should be easy
to track it down and fix it.

By Applying S.O.L.I.D we can achieve all these things that mentioned above.

# What is SOLID?

**SRP** - **S**ingle **R**esponsibility **P**rinciple  
A class should do one thing and therefore it should have only a single reason to change.

**Open-Closed Principle**  
Classes should be open for extension and closed to modification.

**Liskov Substitution Principle**  
Super class should be replacable by it's sub classs.

**Interface Segregation Principle**   
Segregation means keeping things separated, and the Interface Segregation Principle is about separating the interfaces.

**Dependency Inversion Principle**   
Classes should depend upon **interfaces or abstract classes** instead of concrete classes and functions.

## Single Responsibility Principle

**SRP** also helps to have less Merge conflict as everything will be separated and place in separated class, There will
be less merge conflicts.

### Basic Example

![UML Product Class](images/uml_product_class.png)

This `Product` object violates the SRP principle. Product class should not have the business
responsibility like `calculateRetailPrice()` or `calculateBusinessPrice()`

### Solution

Create separate `Calculator` class so changes become easy and introduce less merge conflicts.

![UML Product Class Separation](images/uml_srp_production_solution.png)

so, final result will be this

![UML Final Product Class of SRP](images/uml_final_srp_product.png)

### Another Example with Code

```kotlin
class Invoice(
    val book: Book,
    val quantity: Int,
    val discountRate: Double,
    val taxRate: Double
) {
    private val total: Double

    init {
        total = calculateTotal()
    }

    fun calculateTotal(): Double {
        val price: Double = (book.price - book.price * discountRate) * quantity
        return price * (1 + taxRate)
    }

    // Violation: #1 - printing invoice should not be Invoice responsibility
    fun printInvoice() {
        println(quantity.toString() + "x " + book.name + " " + book.price + "$")
        println("Discount Rate: $discountRate")
        println("Tax Rate: $taxRate")
        println("Total: $total")
    }

    // Violation: #2 - saving file should not be Invoice responsibility
    fun saveToFile(filename: String?) {
        // Creates a file with given name and writes the invoice
    }
}
```
### Solution
Create 2 classes `InvoicePrinter` & `InvoicePersistance` to delegate the separated responsibility

```kotlin
class InvoicePrinter(invoice: Invoice) {
        val invoice: Invoice

        init {
            this.invoice = invoice
        }

        fun print() {
            println(((invoice.quantity + "x " + invoice.book.name).toString() + " " + invoice.book.price).toString() + " $")
            System.out.println("Discount Rate: " + invoice.discountRate)
            System.out.println("Tax Rate: " + invoice.taxRate)
            System.out.println("Total: " + invoice.total + " $")
        }
    }
```

```kotlin
class InvoicePersistence(invoice: Invoice) {
    var invoice: Invoice

    init {
        this.invoice = invoice
    }

    fun saveToFile(filename: String?) {
        // Creates a file with given name and writes the invoice
    }
}
```

```kotlin
class Invoice(
    val book: Book,
    val quantity: Int,
    val discountRate: Double,
    val taxRate: Double
) {
    private val total: Double

    init {
        total = calculateTotal()
    }

    fun calculateTotal(): Double {
        val price: Double = (book.price - book.price * discountRate) * quantity
        return price * (1 + taxRate)
    }
}
```

So `main.kt` function will be
```kotlin

fun main(args: Array<String>) {
    val book = Book(
        name = "Clean Architecture",
        price = 1090.0
    )
    val invoice = Invoice(
        book = book,
        quantity = 1,
        discountRate = 1.2,
        taxRate = 15
    )
    
    val invoicePrinter = InvoicePrinter(invoice)
    invoicePrinter.print()
    
    val invoicePersistence = InvoicePersistence(invoice)
    invoicePersistence.saveToFile()
}

data class Book(
    val name : String,
    val price : Double
)

```

##

