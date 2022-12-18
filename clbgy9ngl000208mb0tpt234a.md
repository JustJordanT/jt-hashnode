# Clean Code

Robert C. Martin, also known as Uncle Bob, is a well-known software engineer and the author of the book "Clean Code: A Handbook of Agile Software Craftsmanship".

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1670615793760/XAJKzKWT_.png align="left")

In "Clean Code", Martin presents a set of principles and practices for writing clean, readable, and maintainable code. He argues that clean code is crucial for creating easy-to-understand, modifying, and extending the software.

Some of the key ideas from "Clean Code" include:

Writing code that is easy to read and understand. This means using clear, descriptive names for variables, functions, and classes and organizing the code logically and consistently.

Keeping functions short and focused. Functions should do one thing and do it well and should not include unnecessary complexity or duplication.

Avoiding duplication. Duplicated code is more difficult to understand and maintain and should be refactored or eliminated whenever possible.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1670615846375/l0uoxS8gC.png align="left")

Writing tests. Tests help ensure that code behaves as expected and can help identify problems early on in the development process.

Overall, "Clean Code" is a valuable resource for software developers looking to improve the quality of their code and write more maintainable, scalable, and reliable software.

Here is an example of clean code in C#:

```plaintext
using System;

namespace eCommerce 
{
    public class Customer
    {
        private readonly string firstName;
        private readonly string lastName;
        private readonly string email;

        public Customer(string firstName, string lastName, string email)
        {
            this.firstName = firstName;
            this.lastName = lastName;
            this.email = email;
        }

        public string GetFullName()
        {
            return $"{firstName} {lastName}";
        }

        public string GetEmail()
        {
            return email;
        }
    }
}
```

This code follows several principles of clean code, including:

Using descriptive and meaningful names for variables and functions. For example, the firstName and lastName variables indicate the information they contain, and the `GetFullName` and `GetEmail` functions are named in a way that clarifies their purpose.

Keeping functions short and focused. The `GetFullName` and `GetEmail` functions are each a single line of code, which makes them easy to understand and maintain.

Avoiding duplication. There is no duplication of code in this example, which makes it easier to understand and maintain.

Writing tests. While not shown in this example, it is important to write tests for this code to ensure that it behaves as expected and to catch any bugs or issues.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1670615898222/cTPEgbfRV.png align="left")

This only scratches the surface but gives a good idea of what we can think about and reflect on when writing our code.

Happy coding, my friends! 🤓