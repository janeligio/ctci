# IX. Interview Questions - Chapter 13 | Java

### 13.1 Private Constructor
> In terms of inheritance, what is the effect of keeping a constructor private?

- So that only a class's children are instantiated. If a constructor is private, its meant for its children.

### 13.2 Return from Finally
> In Java, does the `finally` block get executed if we insert a `return` statement inside the try block of a `try-catch-finally`?

- Yes, it gets executed when the try block exits.

### 13.3 Final, etc.
> What is the difference between `final`, `finally`, and `finalize`?

- final: declaring a variable, method, or class unchangeable
    - like `static final int`
- finally: used in a try-catch black to make sure a segment of code is always executed
- finalize(): this method is called by the garbage colector once it determines that no more references exist

### 13.4 Generics vs. Templates
> Explain the difference between templates in C++ and generics in Java.


### 13.5 TreeMap, HashMap, LinkedHashMap
> Explain the differences between TreeMap, HashMap, and LinkedHashMap. Provide an example of when each one would be best.

### 13.6 Object Reflection
> Explain what object reflection is in Java and why it is useful.

### 13.7 Lambda Expressions
> There is a class Country that has methods getContinent() and getPopulation(). Write a function int getPopulation(List<Country> countries, String continent) that computes the total population of a given continent, given a list of all countries and the name of a continent.

```
int getPopulation(List<Country> countries, String continent) {
    int population = 0;
    for(Country c : countries) {
        if(c.getContinent() == continent) {
            population += c.getPopulation();
        }
    }
    return population;
}
```

### 13.8 Lambda Random
> Using Lambda expressions, write a function List<Integer> getRandomSubset(List<Integer> list) that returns a random subset of arbitrary size. All subsets (including the empty set) should be equally likely to be chosen.

```
List<Integer> getRandomSubset(List<Integer> list) {
    Random rand = new Random();
    int size = rand.getNextInt(list.size());

    int offset = rand.getNextInt(list.size() - size);

    List<Integer> subset = new List<Integer>;

    for(int i = offset; i < offset+size; i++) {
        subset.add(new Integer(list.get(i)));
    }

    return subset;
}
```