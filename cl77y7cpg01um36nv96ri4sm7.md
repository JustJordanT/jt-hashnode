## Day 2 - BigO Notation

## What is BigO Notation?

Simply put, this is a way for us the measure our programs to see how efficient they are; measured by Time (how long) and Space complexity, Space Complexity of an algorithm is the total space taken by the algorithm concerning the input size.    


![E5117505-A740-481D-8F8E-6E0B2D75A4CF.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661353775333/LdSJMZEkp.png align="left")

Looking at this graph, I know you may have already thought about hitting the back button, but trust me, it is not that bad at all; yes, it looks “Mathy” if you even want to consider that a word; but let’s take a deeper dive into what all of this means.

For BigO notation, we want to consider operations and the elements we handle within our program.

### O(1)

One thing to mention is that these notations are pronounced as Ohh-Of-(something); In this case, we have Ohh-of-1, just an FYI. 

Looking at this, we can read this as our program is doing just one thing; let’s take a look.  

```csharp
public static int ReturnList(int[] nums)
{
   return nums[0];
}
```

Reading through our code, we see that we pass in an Array of numbers, then we return the first element in the Array, giving us a BigO notation of `O(1)`; this is also referred to as constant time.

### O(log n)

This notation is pretty common with a lot of Data structures, as we can see below; let’s take a look at a common structure for `O(log n)`; Binary Search Tree; I will be adding some resources for more information on the different types of notations that we are seeing with Big) notation if you are so inclined to dive deeper, for now, we will just be comparing this without code and come to an understanding on how these are correlated.
 
![B6D1D2FC-CAF3-455E-9AD1-8394CB90AC50.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661353798019/iXXF1mxA-.png align="left")

**Binary search tree**, can be thought of as a Divide and concur structure; where the idea is to take an Array and compare the target with the middle of the Array; if this matches, we will return `true,` but if it does not match, we will continue with this process and keep on dividing and comparing until we have our match.

```csharp
		public static object BinarySearchIterative(int[] inputArray, int key)  
		{ 
		  int min = 0;  int max = inputArray.Length - 1; 
		    while (min <=max)  
		    {  
		       int mid = (min + max) / 2;  
		       if (key == inputArray[mid])  
		       {  
		            return true;  
		       }  
		       else if (key < inputArray[mid])  
		       {  
		           max = mid - 1;  
		       }  
		       else  
		       {  
		            min = mid + 1;  
		       }  
		   }  
		   return false;  
		}  

```

### O(n)
O(n) is a linear time complexity where the number of operations grows with the elements in our program. If we take a look at our chart, this will make more sense. 


![E5117505-A740-481D-8F8E-6E0B2D75A4CF.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661353813772/peQaA7Ke_.png align="left")

```csharp
publist static void() loops(int[] nums)
{
		for (int j = 0; j < nums.Length; j++)
		{
  		 Console.WriteLine(nums[j]);
		}
}
```

As we look through the prior code, we can see that this could print only once or 10,000 times just, depending on the number of elements in our Array.

### O(n^2)
This time complexity is the worst that we have seen thus far I wanted to through this one in here since we will see it a lot of the time; this can be called the “brute force” method. Some have said, “Brute Force is the most basic and simplest type of algorithm. A Brute Force Algorithm is the straightforward approach to a problem, i.e., the first approach that comes to mind when seeing the problem.” 

Given the following question. 

> Given an array of integer numbers and an integer target, return indices of the two numbers such that they add up to the target.


We could probably come up with a quick way to find the answer to this question; We could just compare each number in the Array and see if we are hitting our target. That would look something like this.

```csharp
public static int[] TwoSums(int[] nums, int target)
    {
        for (int i = 0; i < nums.Length; i++)
        {
            for (int j = i + 1; j < nums.Length; j++)
            {
                if (nums[i] + nums[j] == target)
                {
                    int[] result = { i, j };
                    return result;
                } 
            }
        }
        return null;
```

Just as we said, we take can compare the 1st element with the 2ed element and so on.

This is only reaching the surface of BigO notation, if you want to dig into this a little deeper I would recommend taking a look at some of the resources below.

As always, happy coding, my friends! 😊 💻 

## Resources

[The Big O Notation – .Net Simplified](https://dotnetsimplified.com/the-big-o-notation/)

[Khan Acadamy BigO Notation](https://www.khanacademy.org/computing/computer-science/algorithms/asymptotic-notation/a/big-o-notation)

[MIT BigO Lecture PDF](http://web.mit.edu/16.070/www/lecture/big_o.pdf)

[Big-O Notation and Algorithm Analysis](https://www.w3schools.in/data-structures/big-o-notation-and-algorithm-analysis)

[analysis-algorithms-big-o-analysis](https://www.geeksforgeeks.org/analysis-algorithms-big-o-analysis/)