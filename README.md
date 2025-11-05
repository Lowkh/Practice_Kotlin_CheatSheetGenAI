# Kotlin Development Cheatsheet

## 1. Basics & Variables

### Variable Declaration
```kotlin
val immutable = "Cannot be reassigned"  // Read-only
var mutable = "Can be reassigned"       // Mutable
const val CONSTANT = "Compile-time constant"
```

### Data Types
```kotlin
val age: Int = 25
val name: String = "John"
val price: Double = 19.99
val isActive: Boolean = true
val items: List<String> = listOf("a", "b", "c")
```

### Type Inference
```kotlin
val auto = 42  // Compiler infers Int
val text = "Hello"  // Compiler infers String
```

## 2. Null Safety

### Nullable Types
```kotlin
val nullable: String? = null
val notNull: String = "Value"

// Safe call operator
val length = nullable?.length  // Returns null if nullable is null

// Elvis operator
val result = nullable ?: "Default"  // Returns "Default" if nullable is null

// Not-null assertion
val length = nullable!!.length  // Throws exception if null
```

### Safe Type Casting
```kotlin
val obj: Any = "String"
if (obj is String) {
    println(obj.length)  // Smart cast
}
```

## 3. Functions

### Basic Function
```kotlin
fun greet(name: String): String {
    return "Hello, $name"
}

// Single expression function
fun add(a: Int, b: Int) = a + b

// Default parameters
fun display(message: String, times: Int = 1) {
    repeat(times) { println(message) }
}

// Named arguments
display(message = "Hi", times = 3)
```

### Extension Functions
```kotlin
fun String.isEmail(): Boolean {
    return this.contains("@")
}

val email = "user@example.com"
println(email.isEmail())  // true
```

### Higher-Order Functions
```kotlin
fun operate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

val result = operate(5, 3) { x, y -> x + y }  // Lambda
```

## 4. Classes & Objects

### Data Classes
```kotlin
data class User(
    val id: Int,
    val name: String,
    val email: String
)

val user = User(1, "John", "john@example.com")
println(user)  // Auto toString()
val (id, name, email) = user  // Destructuring
```

### Regular Classes
```kotlin
class Person(val name: String, var age: Int) {
    init {
        println("$name created")
    }
    
    fun greet() = "Hello, I'm $name"
}

val person = Person("Alice", 30)
```

### Sealed Classes
```kotlin
sealed class Result<out T>
data class Success<T>(val data: T) : Result<T>()
data class Error<T>(val exception: Exception) : Result<T>()
```

### Object & Singleton
```kotlin
object AppConfig {
    val apiUrl = "https://api.example.com"
    fun getTimeout() = 5000
}

AppConfig.apiUrl
```

## 5. Control Flow

### If/Else
```kotlin
val max = if (a > b) a else b
val status = when (age) {
    in 0..12 -> "Child"
    in 13..19 -> "Teen"
    else -> "Adult"
}
```

### Loops
```kotlin
for (i in 1..10) println(i)           // Range
for (i in 10 downTo 1) println(i)     // Reverse
for (i in 1..10 step 2) println(i)    // Step
for (item in items) println(item)     // Iterable

items.forEach { item -> println(item) }
```

### When Expression
```kotlin
val result = when (value) {
    1 -> "One"
    2 -> "Two"
    in 3..5 -> "Three to Five"
    is String -> "String value"
    else -> "Other"
}
```

## 6. Collections

### Lists
```kotlin
val list = listOf(1, 2, 3)              // Immutable
val mutableList = mutableListOf(1, 2, 3)
mutableList.add(4)
list[0]  // Access by index
```

### Maps
```kotlin
val map = mapOf("a" to 1, "b" to 2)
val mutableMap = mutableMapOf<String, Int>()
mutableMap["key"] = 100
map["a"]  // Returns 1
```

### Sets
```kotlin
val set = setOf(1, 2, 3)
val mutableSet = mutableSetOf<Int>()
```

### Collection Operations
```kotlin
val numbers = listOf(1, 2, 3, 4, 5)

numbers.map { it * 2 }              // [2, 4, 6, 8, 10]
numbers.filter { it > 2 }           // [3, 4, 5]
numbers.fold(0) { acc, n -> acc + n }  // 15
numbers.any { it > 4 }              // true
numbers.all { it > 0 }              // true
```

## 7. Coroutines (Basics)

### Launch & Async
```kotlin
GlobalScope.launch {
    delay(1000)
    println("After 1 second")
}

val result = GlobalScope.async {
    delay(1000)
    "Result"
}.await()
```

### Scope Builder
```kotlin
lifecycleScope.launch {
    val data = withContext(Dispatchers.IO) {
        fetchData()  // Background thread
    }
    updateUI(data)  // Main thread
}
```

## 8. Android with Jetpack Compose

### Basic Composable
```kotlin
@Composable
fun Greeting(name: String) {
    Text("Hello, $name!")
}

@Preview
@Composable
fun GreetingPreview() {
    Greeting("World")
}
```

### State Management
```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }
    
    Column {
        Text("Count: $count")
        Button(onClick = { count++ }) {
            Text("Increment")
        }
    }
}
```

### Common Composables
```kotlin
Column {  // Vertical layout
    Text("Line 1")
    Text("Line 2")
}

Row {  // Horizontal layout
    Text("Left")
    Text("Right")
}

Button(onClick = { /* action */ }) {
    Text("Click me")
}

TextField(
    value = text,
    onValueChange = { text = it },
    label = { Text("Enter text") }
)

LazyColumn {  // Scrollable list
    items(items) { item ->
        Text(item)
    }
}
```

## 9. Android Lifecycle

### Activity Lifecycle
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MyApp()  // Jetpack Compose
        }
    }
    
    override fun onStart() {
        super.onStart()
        // Activity becomes visible
    }
    
    override fun onResume() {
        super.onResume()
        // Activity gains focus
    }
    
    override fun onPause() {
        super.onPause()
        // Another activity is taking focus
    }
    
    override fun onStop() {
        super.onStop()
        // Activity is no longer visible
    }
    
    override fun onDestroy() {
        super.onDestroy()
        // Activity is being destroyed
    }
}
```

## 10. String Interpolation & Formatting

### String Templates
```kotlin
val name = "Kotlin"
val version = 1.9
println("$name version $version")
println("${name.length} characters")
```

### Raw Strings
```kotlin
val path = """
    C:\Users\name
    Documents
""".trimIndent()
```

## 11. Scope Functions

### Apply
```kotlin
val person = Person("John", 30).apply {
    age = 25
    println(name)
}
```

### Let
```kotlin
val person: Person? = getPerson()
person?.let {
    println(it.name)
}
```

### Also
```kotlin
val list = mutableListOf(1, 2, 3)
    .also { println("List: $it") }
    .also { it.add(4) }
```

### With
```kotlin
val person = Person("John", 30)
with(person) {
    println("$name is $age years old")
}
```

## 12. Exception Handling

### Try-Catch
```kotlin
try {
    val result = riskyOperation()
} catch (e: IllegalArgumentException) {
    println("Invalid argument: ${e.message}")
} catch (e: Exception) {
    println("Error: ${e.message}")
} finally {
    println("Cleanup")
}

// Try as expression
val result: Int = try {
    "123".toInt()
} catch (e: NumberFormatException) {
    -1
}
```

## 13. Interfaces & Inheritance

### Interface
```kotlin
interface Animal {
    fun makeSound()
    val species: String
}

class Dog : Animal {
    override fun makeSound() = "Woof"
    override val species = "Canis familiaris"
}
```

### Inheritance
```kotlin
open class Vehicle(val brand: String) {
    open fun start() = println("Starting...")
}

class Car(brand: String) : Vehicle(brand) {
    override fun start() = println("Car starting")
}
```

## 14. Delegation

### Class Delegation
```kotlin
interface Printer {
    fun print()
}

class ConsolePrinter : Printer {
    override fun print() = println("Printing...")
}

class Document(printer: Printer) : Printer by printer
```

### Property Delegation
```kotlin
class User {
    val name: String by lazy {
        println("Loading name...")
        "John"
    }
}
```

## 15. Common Patterns

### Builder Pattern
```kotlin
class Coffee {
    var size: String = "Medium"
    var sugar: Int = 0
    var milk: Boolean = false
    
    fun build() = "Coffee($size, sugar=$sugar, milk=$milk)"
}

fun coffee(block: Coffee.() -> Unit) = Coffee().apply(block)

val myCoffee = coffee {
    size = "Large"
    sugar = 2
    milk = true
}
```

### Companion Object
```kotlin
class MyClass {
    companion object {
        const val VERSION = "1.0"
        
        fun create() = MyClass()
    }
}

MyClass.VERSION
MyClass.create()
```

## 16. Testing with Kotlin

### Unit Testing
```kotlin
class CalculatorTest {
    @Test
    fun testAdd() {
        val calc = Calculator()
        assertEquals(4, calc.add(2, 2))
    }
}
```

### Assertions
```kotlin
assertTrue(value)
assertFalse(value)
assertEquals(expected, actual)
assertNull(value)
assertNotNull(value)
```

## 17. Gradle Configuration (App-Level)

### build.gradle.kts
```kotlin
plugins {
    id("com.android.application")
    kotlin("android")
}

android {
    namespace = "com.example.app"
    compileSdk = 34
    
    defaultConfig {
        applicationId = "com.example.app"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }
}

dependencies {
    implementation("androidx.core:core-ktx:1.10.1")
    implementation("androidx.compose.ui:ui:1.5.0")
    testImplementation("junit:junit:4.13.2")
}
```

## 18. Resources & Tips

### Useful Links
- Official Kotlin Docs: kotlinlang.org
- Android Developers: developer.android.com
- Jetpack Compose: developer.android.com/compose

### Best Practices
- Use `val` by default, `var` only when needed
- Leverage null safety features
- Use data classes for simple models
- Prefer extension functions over utilities
- Use coroutines for async operations
- Keep composables pure and stateless when possible
- Utilize smart casts instead of manual casting
- Use appropriate scope functions (apply, let, with, also)

## 19. Quick Reference

| Concept | Kotlin | Use Case |
|---------|--------|----------|
| Immutable Variable | `val` | Default choice |
| Mutable Variable | `var` | When value changes |
| Nullable Type | `String?` | Value can be null |
| Safe Navigation | `obj?.method()` | Avoid NPE |
| Default Value | `?: "default"` | Provide fallback |
| Lambda | `{ x -> x * 2 }` | Higher-order functions |
| Data Class | `data class` | Value objects |
| Sealed Class | `sealed class` | Type-safe enums |
| Coroutine | `launch` / `async` | Async operations |
| Composable | `@Composable` | UI components |

## 20. Common Mistakes to Avoid

### ❌ Don't
```kotlin
var name = "John"  // Use val for immutable data
name = "Jane"

val list: List<String> = mutableListOf()  // Constrains mutability

val result = operation() ?: throw Exception()  // Redundant elvis

obj!!.method()  // Unsafe assertion
```

### ✅ Do
```kotlin
val name = "John"
var mutableName = "John"

val list: MutableList<String> = mutableListOf()

val result = operation() ?: return

obj?.method()  // Safe navigation
```
