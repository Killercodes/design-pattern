Iterator design pattern
===

The Iterator design pattern is a behavioral pattern that provides a way to access the elements of an aggregate object (such as a list) sequentially without exposing its underlying representation. The key idea of the Iterator pattern is to extract the responsibility of accessing and iterating over elements of a collection from the collection itself into an Iterator object, which allows the client to access elements of the collection in a uniform manner regardless of the collection's implementation.


> **Usage examples:** The pattern is very common in C# code. Many frameworks and libraries use it to provide a standard way for traversing their collections.

> **Identification:** Iterator is easy to recognize by the navigation methods (such as next, previous and others). Client code that uses iterators might not have direct access to the collection being traversed.

The Iterator pattern consists of two main components: the Iterator and the Aggregate. The Iterator provides an interface for accessing and traversing the elements of a collection, and the Aggregate defines an interface for creating an Iterator.

Using the Iterator pattern can help to simplify client code and make it more flexible, by decoupling the client code from the specific implementation of the collection being iterated over. It can also make it easier to add new types of collections to an application without having to modify the client code.

Some examples of the Iterator pattern in action include the built-in iterators in programming languages such as Python and Java, as well as the foreach loop in C#.





## Example
```cs
abstract class Iterator : IEnumerator
{
    object IEnumerator.Current => Current();

    // Returns the key of the current element
    public abstract int Key();
    
    // Returns the current element
    public abstract object Current();
    
    // Move forward to next element
    public abstract bool MoveNext();
    
    // Rewinds the Iterator to the first element
    public abstract void Reset();
}

abstract class IteratorAggregate : IEnumerable
{
    // Returns an Iterator or another IteratorAggregate for the implementing
    // object.
    public abstract IEnumerator GetEnumerator();
}

// Concrete Iterators implement various traversal algorithms. These classes
// store the current traversal position at all times.
class AlphabeticalOrderIterator : Iterator
{
    private WordsCollection _collection;
    
    // Stores the current traversal position. An iterator may have a lot of
    // other fields for storing iteration state, especially when it is
    // supposed to work with a particular kind of collection.
    private int _position = -1;
    
    private bool _reverse = false;

    public AlphabeticalOrderIterator(WordsCollection collection, bool reverse = false)
    {
        this._collection = collection;
        this._reverse = reverse;

        if (reverse)
        {
            this._position = collection.getItems().Count;
        }
    }
    
    public override object Current()
    {
        return this._collection.getItems()[_position];
    }

    public override int Key()
    {
        return this._position;
    }
    
    public override bool MoveNext()
    {
        int updatedPosition = this._position + (this._reverse ? -1 : 1);

        if (updatedPosition >= 0 && updatedPosition < this._collection.getItems().Count)
        {
            this._position = updatedPosition;
            return true;
        }
        else
        {
            return false;
        }
    }
    
    public override void Reset()
    {
        this._position = this._reverse ? this._collection.getItems().Count - 1 : 0;
    }
}

// Concrete Collections provide one or several methods for retrieving fresh
// iterator instances, compatible with the collection class.
class WordsCollection : IteratorAggregate
{
    List<string> _collection = new List<string>();
    
    bool _direction = false;
    
    public void ReverseDirection()
    {
        _direction = !_direction;
    }
    
    public List<string> getItems()
    {
        return _collection;
    }
    
    public void AddItem(string item)
    {
        this._collection.Add(item);
    }
    
    public override IEnumerator GetEnumerator()
    {
        return new AlphabeticalOrderIterator(this, _direction);
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code may or may not know about the Concrete Iterator
        // or Collection classes, depending on the level of indirection you
        // want to keep in your program.
        var collection = new WordsCollection();
        collection.AddItem("First");
        collection.AddItem("Second");
        collection.AddItem("Third");

        Console.WriteLine("Straight traversal:");

        foreach (var element in collection)
        {
            Console.WriteLine(element);
        }

        Console.WriteLine("\nReverse traversal:");

        collection.ReverseDirection();

        foreach (var element in collection)
        {
            Console.WriteLine(element);
        }
    }
}

```

## Example 2
The Iterator design pattern is a behavioral pattern that provides a way to access the elements of an aggregate object without exposing its underlying implementation. It provides a standard interface for traversing a collection of objects, without having to expose the underlying implementation details of the collection.

- The Iterator pattern is based on the concept of iterators, which are objects that can be used to traverse a collection of elements. An iterator provides a way to access the elements of a collection one by one, without having to know the details of how the collection is implemented.

- The main advantage of using the Iterator pattern is that it decouples the client code from the implementation of the collection, making it possible to change the implementation of the collection without affecting the client code.

- The Iterator pattern consists of three main components: the Iterator interface, the ConcreteIterator class, and the Aggregate interface. The Iterator interface defines a standard interface for iterating over a collection of objects. The ConcreteIterator class implements the Iterator interface and provides a concrete implementation of the iteration algorithm. The Aggregate interface defines a standard interface for creating iterators over a collection of objects.

- The Iterator pattern is commonly used in modern programming languages, such as Java and C#, to provide a way to traverse collections of objects. It is particularly useful when dealing with large collections of objects, where it is impractical to load all the objects into memory at once.

Some common use cases for the Iterator pattern include:

1. Traversing a collection of objects in a specific order
2. Providing a standard way to iterate over the elements of a collection
3. Hiding the implementation details of a collection from client code
4. Supporting multiple iterations over the same collection at the same time

Overall, the Iterator pattern is a powerful and flexible pattern that can be used to solve a wide range of problems related to traversing collections of objects.


### C#
```cs
using System;
using System.Collections;

// Define the Book class
class Book
{
    public string Title { get; set; }
    public string Author { get; set; }
    public DateTime PublicationDate { get; set; }
}

// Define the BookCollection class
class BookCollection : IEnumerable
{
    private ArrayList _books = new ArrayList();

    public void Add(Book book)
    {
        _books.Add(book);
    }

    // Create an iterator object that can be used to iterate over the collection
    public IEnumerator GetEnumerator()
    {
        return new BookIterator(_books);
    }

    // Define the BookIterator class
    private class BookIterator : IEnumerator
    {
        private ArrayList _books;
        private int _current = -1;

        public BookIterator(ArrayList books)
        {
            _books = books;
        }

        public bool MoveNext()
        {
            _current++;
            return _current < _books.Count;
        }

        public void Reset()
        {
            _current = -1;
        }

        // Get the current book
        public object Current
        {
            get { return _books[_current]; }
        }
    }
}

// Test the BookCollection class and BookIterator class
class Program
{
    static void Main(string[] args)
    {
        BookCollection collection = new BookCollection();

        collection.Add(new Book { Title = "The Catcher in the Rye", Author = "J.D. Salinger", PublicationDate = new DateTime(1951, 7, 16) });
        collection.Add(new Book { Title = "To Kill a Mockingbird", Author = "Harper Lee", PublicationDate = new DateTime(1960, 7, 11) });
        collection.Add(new Book { Title = "The Great Gatsby", Author = "F. Scott Fitzgerald", PublicationDate = new DateTime(1925, 4, 10) });

        // Iterate over the collection using the iterator object
        foreach (Book book in collection)
        {
            Console.WriteLine("{0} by {1} ({2})", book.Title, book.Author, book.PublicationDate.ToString("d"));
        }

        Console.ReadKey();
    }
}


```

In this example, we define the Book class with properties for the title, author, and publication date. We also define the BookCollection class, which implements the IEnumerable interface. The BookCollection class has an Add method to add books to the collection and a CreateIterator method to create an iterator object that can be used to iterate over the collection.

The BookIterator class is a private nested class within the BookCollection class. It implements the IEnumerator interface and defines the MoveNext method to move to the next item in the collection and the Reset method to reset the iterator. The Current property returns the current book.

In the Main method, we create a BookCollection object and add some books to it. We then use a foreach loop to iterate over the collection using the iterator object returned by the GetEnumerator method of the BookCollection class.

### javascript

```js
// Iterator interface
class Iterator {
  constructor(items) {
    this.index = 0;
    this.items = items;
  }

  first() {
    this.reset();
    return this.next();
  }

  next() {
    return this.items[this.index++];
  }

  hasNext() {
    return this.index <= this.items.length;
  }

  reset() {
    this.index = 0;
  }

  current() {
    return this.items[this.index];
  }
}

// Aggregate interface
class Aggregate {
  createIterator() {}
}

// Concrete aggregate
class ConcreteAggregate extends Aggregate {
  constructor() {
    super();
    this.items = [];
  }

  createIterator() {
    return new Iterator(this.items);
  }

  addItem(item) {
    this.items.push(item);
  }
}

// Usage
const concreteAggregate = new ConcreteAggregate();
concreteAggregate.addItem('Item 1');
concreteAggregate.addItem('Item 2');
concreteAggregate.addItem('Item 3');

const iterator = concreteAggregate.createIterator();
while (iterator.hasNext()) {
  console.log(iterator.next());
}

```


### Python

```py
# Iterator interface
class Iterator:
    def __init__(self, items):
        self.index = 0
        self.items = items

    def first(self):
        self.reset()
        return self.next()

    def next(self):
        value = self.items[self.index]
        self.index += 1
        return value

    def has_next(self):
        return self.index <= len(self.items)

    def reset(self):
        self.index = 0

    def current(self):
        return self.items[self.index]

# Aggregate interface
class Aggregate:
    def create_iterator(self):
        pass

# Concrete aggregate
class ConcreteAggregate(Aggregate):
    def __init__(self):
        self.items = []

    def create_iterator(self):
        return Iterator(self.items)

    def add_item(self, item):
        self.items.append(item)

# Usage
concrete_aggregate = ConcreteAggregate()
concrete_aggregate.add_item('Item 1')
concrete_aggregate.add_item('Item 2')
concrete_aggregate.add_item('Item 3')

iterator = concrete_aggregate.create_iterator()
while iterator.has_next():
    print(iterator.next())

```

