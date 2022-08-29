## Day 3 - Array

Let’s talk about arrays; arrays can be thought of like boxes, almost like a bento lunch box; we can have many different compartments that we have access to; for storing and retrieving data.

![A89C96BD-F396-4FC5-B9FF-CE33B97CFBB5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661785071922/u2z2x-89N.png align="left")

From a BigO standpoint, we can see where arrays stand from a Time and Space complexity.


![EBA1885F-793A-4D79-A758-EBC3E289B657.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661784975841/RYK56Nt1J.png align="left")

## Array Types
With arrays, we can have two different types.

* Static
* Dynamic

One thing to note is that we can have single-dimensional arrays that are static or dynamic and also multi-dimensional arrays that can be static or dynamic.

### Static


![709E168B-5018-41C5-B126-61D893580DEC.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661784998043/bAjJbnKm7.png align="left")

Static arrays have a fixed size that they can use. Let’s take a look at how we can use static arrays. 

```csharp
// Static Arrays
string[] easternStates = new string[5];

// initializing the array
easternStates[0] = "NY";
easternStates[1] = "FL";
easternStates[2] = "NC";
easternStates[3] = "SC";
easternStates[4] = "NJ";

```

We can also initialize an array up front as well. 

```csharp
string[] westernStates = { "California", "Oregon", "Arizona" };
```

### Dynamic

Dynamic arrays look the same as single-dimensional arrays, but the array size is not fixed to a given integer. As we saw prior, we can set a static array to size; with dynamic arrays, this is not needed; this is now handled at runtime.

It is good to know that static or dynamic arrays do not differ in time complexity. Specific operations like resize() increase or decreases the size of the Dynamic Array, but in doing so, it needs no extra memory. thus, the time complexity of resizing an array is O(1).

```csharp
ArrayList numbers = new ArrayList
    {"thing1", "thing2", "thing3"};

numbers.Add("thing4");

foreach (var i in numbers)
{
    Console.WriteLine($"element: {i}");
}
```

### Single Dimension

In computer science, an array data structure is a data structure consisting of a collection of elements, each identified by at least one array index. An array is stored in such a way that the index of each element can be computed from its index tuple by a mathematical formula.
The simplest type of data structure is a linear array, also called a one-dimensional array or Sigle-Dimensional Array.

As an example, a single-dimensional array may look pretty familiar because we tend to use them a lot.

```csharp
string[] westernStates = { "California", "Oregon", "Arizona" };
```

### Multi Dimension

We can think of a multi-dimensional array just like a single-dimensional one but
where we stack multiple arrays on one another, giving us rows and columns.

From a mathematical standpoint, these are called Matrix.

In mathematics, a matrix (plural matrices) is a rectangular array or table of numbers, or expressions, arranged in rows and columns, which is used to represent a mathematical object or a property of such an object.

```csharp
int[,] nums = { { 1, 2, 3 }, { 4, 5, 6 }, { 7, 8, 9 } };
public static int GetNum(int i, int j, int[,])
    {
        return nums[i,j];
    }
```


## Resources

While using arrays, be sure to take a look at your language documentation for best practices for using arrays.

Look at this array data sheet I have made that you can download for free, hope this helps you in your DSA journey; if not, at least I got to make it for you!

![67041192-F56F-44F9-8C53-0AF2937509F4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661785014171/279wTUv4a.png align="left")
[Array Data Sheet](https://store.justjordant.com/l/array-datasheet) Download link.

Now that we have reviewed arrays, try completing the following questions using arrays.

[Two Sum - LeetCode](https://leetcode.com/problems/two-sum/)


### Links
[Arrays - C# Programming Guide | Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/)

[Array data structure - Wikipedia](https://en.wikipedia.org/wiki/Array_data_structure#cite_note-andres-2)

[Multidimensional Arrays - C# Programming Guide | Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/multidimensional-arrays?source=recommendations)

[Single-Dimensional Arrays - C# Programming Guide | Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/arrays/single-dimensional-arrays)