Skills:
- [main function](#main-function)
- [cout](#cout)
- [arrows](#arrows)
- [if/else](#ifelse)
- [Random generator](#random-generator)
- [Logical operator short-circuit evaluation](#logical-operator-short-circuit-evaluation)
- [Input auto-conversion](#input-auto-conversion)
- [Increment](#increment)
- [For loop](#for-loop)
<br>

### main function
- Special function which must have return type *int*
- does not need an explicit return, automatically has implicit return value *0*

### cout
- *cout* stands for ***c**haracter out*

### arrows
- direction of double arrow *<<* determined by input/output
- element acts as input to element in direction of arrow

### if/else

``` C++
if (condition) {
	\\ statement
}
else if (condition2) {
	\\ statement
}
else {
	\\ statement
}
```

### Random generator
- Random number generator requires importing *cstdlib* library
- Need to seed random generator before each generation

``` C++
#include <cstdlib>

int main() {
	srand(time(NULL)); // seeds generator
	int rnum = std::rand() % 10;
}
```

### Logical operator short-circuit evaluation
- When using *and*, if the first statement evaluates to false then the second statement will not be evaluated

### Input auto-conversion
- Inputs are automatically read as string or int
- Int values 1/0 can be read as boolean if saved as a boolean value
- If converting from string to boolean, require explicit assignment statements using if/else

### Increment
- C++ uses incrementation like Java
- x++ increments after processing
- ++x increments before processing

### For loop
```C++
for (type var1=value; var1 condition;var1 change) {
	statement;
}
```
