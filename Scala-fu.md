# Scala-fu
Collection of things I learned while studying Scala over the summer
---

<hr>

## FizzBuzz
Two implementations, but they work the same way. Big thank you to [Xe](https://github.com/Xe) for her suggestion to use tuples!

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
// And then...
$(1 to 100).foreach(fizzbuzz)

$ 1
  2
  fizz
  ...
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

---

## Dealing with Java shenanigans

Occasionally you have to deal with Big Bro Java, such as using libraries made for Java. And very frequently you will want to use Scala's types, not Java's, for handling that sort of data.

For now my first encounter with this problem was dealing with configuration files. For this example, let's take parsing YAML into sensible data for a Scala program to use. To keep it simple, we'll make a `(String -> String)` map

### Parsing YAML 
```yaml
# YAML "configuration file"
key: "value"
another key: "another value" 
```

Maybe I missed a library that parses YAML directly into Scala(If there is PLEASE let me know), but in this case, I used [snakeYaml](https://bitbucket.org/asomov/snakeyaml/) to parse my file

After you add the `.jar` to your Java `$CLASSPATH` you can import it to your java/scala files with the package `org.yaml.snakeyaml`. The bitbucket documentation has more detailed info.

 - Note on classpath: Its location varies by platform, however on macOS it came to my attention that it doesn't explicitly exist, but instead it is `~/Library/Java/Extensions` (user library). If you want *ALL* your java programs to have access, use the system directory, `/Library/Java/Extensions`.

#### Scalala! We're off!

1. Import your Java file IO libraries and YAML parser
```scala 
import java.io.{File, FileInputStream, InputStream}
import org.yaml.snakeyaml.Yaml
```
2. Get your input file stream ready
```scala
val input: InputStream = new FileInputStream(new File("path/to/yaml"))
```
3. Feed it into your parser
```scala
val yaml: Yaml = new Yaml() // Yeems
val data = yaml.load(input) // Load 'er up
    // > data: Object = {key=value, another key=another value}
```

Sweet! You have an Object... Now what?

Turn it into a map!
```scala
val myMap = data.asInstanceOf[Map[String, String]]
```

`> java.lang.ClassCastException: java.util.LinkedHashMap cannot be cast to scala.collection.immutable.Map`

Nope, not as easy as I was expecting, either. And I'm pretty sure juggling Java types in the middle of a Scala application is not something you want to be doing, unless you like incessant pain and suffering.

Using java Map classes, will give you something a little more useful
```scala
val myMap = data.asInstanceOf[java.util.Map[String,String]]
```
Now you have a Java map. One step at a time! Now you need to get yourself some conversion tools so you can change the sucker into a proper Scala map
```scala
import scala.collection.JavaConverters._
```
And finally:
```scala
// This is the longer version, but guarantees you're putting in a String value for each key.
val myScalaMap = myMap.asScala.mapValues(_.asScala.toString) 

// Quick and easy version
val myScalaMap = myMap.asScala
```

Both these methods will yield a proper Scala `Map[String, String]`. In the case of config files, you'll want to check that the values provided are correct, and so on. But that is a lot simpler now that you have sane types to deal with and you can say bye to Java (for now)

---

*More coming soon*
---