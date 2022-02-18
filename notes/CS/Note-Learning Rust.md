# Learning Rust


## Getting started

### Creating new project
```rust
$ cargo new project_name
```

### `rustc`
- More tedious way to run the program
```rust
$ rustc main.rs // compling the program 
$ ./main // run it
```

### `cargo`
- More efficient way to run the program

```rust
cargo new project_name
cargo build // creates an executable file in target/debug/project_name
cargo run // compile the code and then run the resulting executable all in one command
cargo check // uickly checks your code to make sure it compiles but doesn’t produce an executable

cargo build --release // compile it with optimizations; create an executable in target/release instead of target/debug
```

- We can build a project using cargo build or cargo check.
- We can build and run a project in one step using cargo run.
- Instead of saving the result of the build in the same directory as our code, Cargo stores it in the target/debug directory.

## Programming a Guessing Game

```rust
use std::io;
use std::cmp::Ordering;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1, 101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin().read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

## Common Programming Concepts

### Variables and Mutability

#### Differences between variables and constants
1. you aren’t allowed to use mut with constants. Constants aren’t just immutable by default—they’re always immutable.
2. You declare constants using the `const` keyword instead of the `let` keyword, and the type of the value must be annotated.
    ```rust
    const MAX_POINTS: u32 = 100_000;
    ```
3. Constants can be declared in any scope, including the global scope, which makes them useful for values that many parts of code need to know about.
4. constants may be set only to a constant expression, not the result of a function call or any other value that could only be computed at runtime.


- Constants are valid for the entire time a program runs, within the scope they were declared in, making them a useful choice for values in your application domain that multiple parts of the program might need to know about,

#### Shadowing
- declare a new variable with the same name as a previous variable, and the new variable shadows the previous variable. Rustaceans say that the first variable is *shadowed* by the second, which means that the second variable’s value is what appears when the variable is used. 
    ```rust
    fn main() {
      let x = 5;

      let x = x + 1;

      let x = x * 2;

      println!("The value of x is: {}", x);
  }
    ```
- Shadowing is different from marking a variable as `mut`, because we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword. 

- The other difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name.
    ```rust
    let spaces = "   ";
    let spaces = spaces.len();
    ```
  - Shadowing thus spares us from having to come up with different names, such as `spaces_str` and `spaces_num`; instead, we can reuse the simpler `spaces` name. 



### Data Types
- Keep in mind that Rust is a *statically typed* language, which means that it must know the types of all variables at compile time.

#### Scalar Types
- A *scalar type* represents a single value. Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters. 

##### Integer Types
- An *integer* is a number without a fractional component.
- Each variant can be either *signed* or *unsigned* and has an explicit size. Signed and unsigned refer to whether it’s possible for the number to be negative or positive—in other words, whether the number needs to have a sign with it (signed) or whether it will only ever be positive and can therefore be represented without a sign (unsigned).
- Each signed variant can store numbers from $-(2^{n - 1})$ to $2^{n - 1} - 1$ inclusive, where n is the number of bits that variant uses.

##### Floating-Point Types
Rust also has two primitive types for *floating-point numbers*, which are numbers with decimal points. Rust’s floating-point types are f32 and f64, which are 32 bits and 64 bits in size, respectively. The default type is f64 because on modern CPUs it’s roughly the same speed as f32 but is capable of more precision.

##### Numeric Operations



##### The Boolean Type
- Booleans are one byte in size. 


##### The Character Type
- Rust’s `char` type is the language’s most primitive alphabetic type, and the following code shows one way to use it. (Note that `char` literals are specified with single quotes, as opposed to string literals, which use double quotes.)
- Rust’s `char` type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more than just ASCII. 
- Unicode Scalar Values range from `U+0000` to `U+D7FF` and `U+E000` to `U+10FFFF` inclusive. However, a “character” isn’t really a concept in Unicode, so your human intuition for what a “character” is may not match up with what a `char` is in Rust. 


#### Compound Types
- *Compound types* can group multiple values into one type. Rust has two primitive compound types: **tuples** and **arrays**.

##### The Tuple Type
- A tuple is a general way of grouping together some number of other values with a variety of types into one compound type. Tuples have a fixed length: once declared, they cannot grow or shrink in size.

- We create a tuple by writing a comma-separated list of values inside parentheses. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same. 
  ```rust
  fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
  }
  ```

- To get the individual values out of a tuple, we can use pattern matching to destructure a tuple value (called *destructuring*):
  ```rust
  fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
  }
  ```

##### The Array Type
- Unlike a tuple, every element of an array must have the **same type**. Arrays in Rust are different from arrays in some other languages because arrays in Rust have a fixed length, like tuples.

- You would write an array’s type by using square brackets, and within the brackets include the type of each element, a semicolon, and then the number of elements in the array
  ```rust
  let a: [i32; 5] = [1, 2, 3, 4, 5];
  ```
  
- An array is a single chunk of memory allocated on the stack. You can access elements of an array using indexing,
    ```rust
    fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
    }
    ```

### Functions


### Comments


### Control Flow

















<img src="https://doc.rust-lang.org/book/img/ferris/panics.svg" width="400">