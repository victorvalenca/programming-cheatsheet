# Scala-fu
**Collection of things I learned while studying Scala over the summer**

<hr>

## FizzBuzz
Two implementations, but they work the same way. Big thank you to @Xe for her suggestion to use tuples!

#### My version:
```scala
def fizzbuzz(i:Int) = {
    (i % 3, i % 5) match {
    case (0,0) => println("fizzbuzz")
    case (_,0) => println("buzz")
    case (0,_) => println("fizz")
    case (_,_) => println(i)
    }
}
```

#### Xe's version:
```scala
1 to 100 foreach { n =>
    println((n % 3, n % 5) match {
        case (0,0) => "fizzbuzz"
        case (_,0) => "buzz"
        case (0,_) => "fizz"
        case (_,_) => n
    })
}
```

**More coming soon**