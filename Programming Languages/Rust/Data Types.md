In [[Rust]], there are many data types that can be used for variables or other functions. Here is a list of the most important.

https://doc.rust-lang.org/std/index.html#primitives

# Numbers

## Signed integers
| Rust variable | Minimum value                            | Maximum value                           |
| ------------- | ---------------------------------------- | --------------------------------------- |
| i8            | -128                                     | 127                                     |
| i16           | -32768                                   | 32767                                   |
| i32           | -2147483648                              | 2147483647                              |
| i64           | -9223372036854775808                     | 9223372036854775807                     |
| i128          | -170141183460469231731687303715884105728 | 170141183460469231731687303715884105727 |

## Unsigned integers

| Rust variable | Minimum value | Maximum value                           |
| ------------- | ------------- | --------------------------------------- |
| u8            | 0             | 255                                     |
| i16           | 0             | 65535                                   |
| i32           | 0             | 4294967295                              |
| i64           | 0             | 18446744073709551615                    |
| i128          | 0             | 340282366920938463463374607431768211455 |

## Floats

| Rust variable | Description                                                                             |
| ------------- | --------------------------------------------------------------------------------------- |
| f32           | A 32-bit floating point that is used to calculate decimal numbers.                                                                 |
| f64           | A 64-bit floating point that is more accurate because it uses twice as many bits as f32 |

## Other

| Rust variable | Description                            | Example                                             |
| ------------- | -------------------------------------- | --------------------------------------------------- |
| isize         | Signed integer based on bit platform   | A 32-bit platform would be a 32-bit signed integer  |
| usize         | Unsigned integer based on bit platform | A 64-bit platform would be a 64-bit unsigned integer |

# Characters

| Rust variable | Description                                                                          |
| ------------- | ------------------------------------------------------------------------------------ |
| String        | A growable UTF-8 type that holds characters                                          |
| &str          | A reference to a string that holds only the data. Is immutible.                      |
| char          | A 32-bit Unicode value that can be used for Unicode or single character declarations |



# Others

| Rust variable | Description                                                                                                      |
| ------------- | ---------------------------------------------------------------------------------------------------------------- |
| array         | An array holds a list of values that can be accesed by moving up and down the list                               |
| bool          | Indicates true or false. This can be an integer with true being `1` and false being `0`                          |
| fn            | This holds code in it that can be called upon to run.                                                            |
| *             | A raw pointer doesn't reference the value, but obtains the value by looking at the memory address                |
| &             | A reference or string slice references the value of a UTF-8 string, but doesn't use the memories address         |
| &             | A slice can reference any type of sequence data such as an array or a vector.                                    |
| ())           | Contains a fixed sized collection of values, can be of different types.                                          |
| ()            | Holding no data, a unit is used to indicate a function or expression has no meaningful result or value to return |
