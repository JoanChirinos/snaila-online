# How to Scala
### by Joan Chirinos

## History of Scala
* Scala is a relatively new language
* ‘Twas designed in 2001 by Martin Odersky at École Polytechnique Fédérale de Lausanne (Switzerland)
* In late 2003ish, it was released on the Java platform (runs in the JVM I believe) and on .NET (Microsoft thing) in 2004
* Version 2.0 was released in 2006
* Finally, the Scala team won a 5 year research grant in 2011

## What is Scala
* General purpose, high-level language with strong static typing
* Supports functional programming and OOP
* It was meant to be Java but better (is it? We’ll see…)
* Lazy evaluation
* Operator overloading
* Does not have checked exceptions
  * In Java, if a method could throw certain exceptions (let’s say IOException), it must be declared in the signature
    * e.g. `public static void foo() throws IOException { //code }`

## Why should I use Scala over Java?
* Scala is more syntactically flexible
  * One of the more useful bois is that Scala recognizes squiggly braces as parameter lists. This can help split some params from others, and allows for better multiline parameters
* Unified type system
  * Everything, including value types like int and boolean, inherit class ‘Any’. You can also define new value types
* for-expressions rather than foreach loops
  * This allows for handy dandy list comprehension-type things!
* It’s very functional (haha get it?)
  * Everything is an expression (every statement returns something)

## How does it compare to the other JVM languages (Clojure, Groovy)
* Scala is typically compared with Clojure and Groovy.
* Clojure and Groovy are both dynamically typed
* Groovy is strongly object oriented, and it aims to reduce verbosity
* Clojure is more strongly functional
* Scala is a happy (albeit weird and—at times–a slightly difficult marriage of the two)
* Learning Scala can be hard because of the various advanced features (which I likely will not delve too deep into)

## Hello world!
```scala
object HelloWorld {
  def main(args: Array[String]): Unit = {
    println("Hello, world!")
  }
}
```

Here is a "Hello, world" program in Scala. I will break it into parts and explain it

* `object HelloWorld` is similar to writing a class in Java
* `def main(args: Array[String]): Unit = {...}` reads as "define the method _main_ which has a single argument, _args_, defined as an Array of Strings. This method returns Unit. Unit is similar to _void_ in other languages (Java, C, etc.)
* `println("Hello, world!")` tells the program to print `"Hello, world!"` then a new-line character onto the terminal

## General Scala Demo!
```scala
object GeneralDemo {
```
```scala
  // we can define functions inside functions for nice recursion
  // here, we define factorial with a loopy recursive fxn inside
  def factorial(n: Int): Int = {
    def go(n: Int, acc: Int): Int =
      if (n <= 0) acc
      else go(n-1, n*acc)

    go(n, 1)
  }
```
Defining a function within another function leads to nice recursion without messing up the larger namespace

```scala

  // now here we print a formatted factorial statement thing
  def formatFactorial(n: Int): String = {
    val msg = "The factorial of %d is %d."
    msg.format(n, factorial(n))
  }
```
Here we see that we can easily format strings to print out the results of some function
```scala
  // we can also define something like the fib sequence like this
  def fib(n: Int): Int = {
    def loop(n: Int, prev: Int, cur: Int): Int =
      if (n == 0) prev
      else loop(n - 1, cur, prev + cur)

    loop(n, 0, 1)
  }
```
Here is another instance of nice recursion to generate meaningful values
```scala
  // now we wanna print this too. We can make a more general print function
  // like so
  // This takes a function name, an int, and a function
  def formatResult(name: String, n: Int, f: Int => Int) = {
    val msg = "The %s of %d is %d."
    msg.format(name, n, f(n))
  }
```
Here it gets cool! We can give a function as an argument to make a multi-purpose print function. This defines the variable `f` as a function which takes an `Int` and returns an `Int`
```scala
  // let's see some polymorphism!
  // Find first element x of type A in an array of As
  // In english, this is:
  // polyFindFirst[generic A](
  //    arr: array of type A, p: equality fxn for value we're looking for
  // )
  def polyFindFirst[A](arr: Array[A], p: A => Boolean): Int = {
    def loop(n: Int): Int =
      if (n >= arr.length) -1
      else if (p(arr(n))) n
      else loop(n + 1)

    loop(0)
  }
```
This is Scala's way of doing polymorphism. We also see another example of giving a function as an argument, defining variable `p` as a function which takes an element of type `A` and returns a `Boolean`
```scala
  // function to check if array of As is sorted given a > fxn
  def isSorted[A](arr: Array[A], gt: (A, A) => Boolean): Boolean = {
    def loop(n: Int): Boolean =
      if (n >= arr.length - 1) true
      else if (gt(arr(n), arr(n+1))) false
      else loop(n+1)

    loop(0)
  }

  def formatArr[A](arr: Array[A]): String = {
    def loop(n: Int): String =
      if (n >= arr.length) ""
      else {
        val msg = "%s, %s"
        msg.format(arr(n).toString, loop(n+1))
      }

    loop(0)
  }

  def formatSort[A](arr: Array[A], gt: (A, A) => Boolean): String = {
    val msg = "%s is sorted: %b"
    msg.format(formatArr(arr), isSorted(arr, gt))
  }
}
```
Here there are just a couple more examples of basic functional programming capabilities in Scala, along with nice recursion

## Scala vs. Java Demo

Scala can make some code very clean and short. Here, we take some sorting algorithms as examples.

### Quicksort

Here is quicksort implemented in Scala
```scala
def quicksort(arr: List[Int]): List[Int] = {
  if (arr.length <= 1) arr
  else {
    val pivot = arr(arr.length / 2)
    quicksort(arr.filter(_ < pivot)) :::
      arr.filter(_ == pivot) :::
      quicksort(arr.filter(_ > pivot))
  }
}
```

And here it's implemented in Java
```java
int partition(int arr[], int low, int high) {
  int pivot = arr[high];
  int i = (low-1); // index of smaller element
  for (int j=low; j<high; j++) {
    if (arr[j] <= pivot) {
      i++;
      int temp = arr[i];
      arr[i] = arr[j];
      arr[j] = temp;
    }
  }
  int temp = arr[i+1];
  arr[i+1] = arr[high];
  arr[high] = temp;
  return i+1;
}

void sort(int arr[], int low, int high) {
  if (low < high) {
    int pi = partition(arr, low, high);
    sort(arr, low, pi-1);
    sort(arr, pi+1, high);
  }
}
```

As we can see, Scala's implementation is much shorter and cleaner than Java. It's very easily readable, and doesn't take too long to understand. In Java, it would take a while to understand what each chunk of code does.

### Mergesort

Mergesort is a little longer, but it is still very short and clean in Scala. It also shows the power of Scala's easy pattern maching
```scala
def merge(left: List[Int], right: List[Int]): List[Int] = (left, right) match {
  case(left, Nil) => left
  case(Nil, right) => right
  case(leftHead :: leftTail, rightHead :: rightTail) =>
    if (leftHead < rightHead) leftHead::merge(leftTail, right)
    else rightHead :: merge(left, rightTail)
}

def mergesort(list: List[Int]): List[Int] = {
  val n = list.length / 2
  if (n == 0) list // i.e. if list is empty or single value, no sorting needed
  else {
    val (left, right) = list.splitAt(n)
    merge(mergesort(left), mergesort(right))
  }
}
```

Java's mergesort is very verbose and clunky
```java
void merge(int arr[], int l, int m, int r) {
  int n1 = m - l + 1;
  int n2 = r - m;
  int L[] = new int [n1];
  int R[] = new int [n2];
  for (int i=0; i<n1; ++i)
      L[i] = arr[l + i];
  for (int j=0; j<n2; ++j)
      R[j] = arr[m + 1+ j];
  int i = 0, j = 0;
  int k = l;
  while (i < n1 && j < n2) {
      if (L[i] <= R[j]) {
          arr[k] = L[i];
          i++;
      }
      else {
          arr[k] = R[j];
          j++;
      }
      k++;
  }
  while (i < n1) {
      arr[k] = L[i];
      i++;
      k++;
  }
  while (j < n2) {
      arr[k] = R[j];
      j++;
      k++;
  }
}
void sort(int arr[], int l, int r) {
  if (l < r) {
      int m = (l+r)/2;
      sort(arr, l, m);
      sort(arr , m+1, r);
      merge(arr, l, m, r);
  }
}
```

## Why should you care about Scala?
* Though I don’t expect anyone in this class to pick up Scala and use it for SoftDev (we have to use Python, fellas), it is a good language to have in your toolbelt!
* You can also apply your SD knowledge to Scala since there are web frameworks out there that use Scala as the backend!
* It’s just something cool y’all can experiment with.
* Also, since it’s strongly typed and ran on our favorite virtual machine, I’m assuming it might be a tad faster than Python + Flask
