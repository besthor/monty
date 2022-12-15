The Monty language

Monty 0.98 is a scripting language that is first compiled into Monty byte codes (Just like Python). It relies on a unique stack, with specific instructions to manipulate it. The goal of this project is to create an interpreter for Monty ByteCodes files.



Monty byte code files



Files containing Monty byte codes usually have the .m extension. Most of the industry uses this standard but it is not required by the specification of the language. There is not more than one instruction per line. There can be any number of spaces before or after the opcode and its argument:



julien@ubuntu:~/monty$ cat -e bytecodes/000.m

push 0$

push 1$

push 2$

  push 3$

                   pall    $

push 4$

    push 5    $

      push    6        $

pall$

julien@ubuntu:~/monty$

Monty byte code files can contain blank lines (empty or made of spaces only, and any additional text after the opcode or its required argument is not taken into account:



julien@ubuntu:~/monty$ cat -e bytecodes/001.m

push 0 Push 0 onto the stack$

push 1 Push 1 onto the stack$

$

push 2$

  push 3$

                   pall    $

$

$

                           $

push 4$

$

    push 5    $

      push    6        $

$

pall This is the end of our program. Monty is awesome!$

julien@ubuntu:~/monty$

The monty program



Usage: monty file

where file is the path to the file containing Monty byte code

If the user does not give any file or more than one argument to your program, print the error message USAGE: monty file, followed by a new line, and exit with the status EXIT_FAILURE

If, for any reason, it’s not possible to open the file, print the error message Error: Can't open file <file>, followed by a new line, and exit with the status EXIT_FAILURE

where <file> is the name of the file

If the file contains an invalid instruction, print the error message L<line_number>: unknown instruction <opcode>, followed by a new line, and exit with the status EXIT_FAILURE

where is the line number where the instruction appears.

Line numbers always start at 1

The monty program runs the bytecodes line by line and stop if either:

it executed properly every line of the file

it finds an error in the file

an error occured

If you can’t malloc anymore, print the error message Error: malloc failed, followed by a new line, and exit with status EXIT_FAILURE.

You have to use malloc and free and are not allowed to use any other function from man malloc (realloc, calloc, …)
Usage

All the files are compiled in the following form:



 gcc -Wall -Werror -Wextra -pedantic *.c -o monty.

To run the program:



 ./monty bytecode_file
gcc -Wall -Werror -Wextra -pedantic *.c -o monty

Available operation codes:



Opcode	Description

push	Pushes an element to the stack. e.g (push 1 # pushes 1 into the stack)

pall	Prints all the values on the stack, starting from the to of the stack.

pint	Prints the value at the top of the stack.

pop	Removes the to element of the stack.

swap	Swaps the top to elements of the stack.

add	Adds the top two elements of the stack. The result is then stored in the second node, and the first node is removed.

nop	This opcode does not do anything.

sub	Subtracts the top two elements of the stack from the second top element. The result is then stored in the second node, and the first node is removed.

div	Divides the top two elements of the stack from the second top element. The result is then stored in the second node, and the first node is removed.

mul	Multiplies the top two elements of the stack from the second top element. The result is then stored in the second node, and the first node is removed.

mod	Computes the remainder of the top two elements of the stack from the second top element. The result is then stored in the second node, and the first node is removed.

#	When the first non-space of a line is a # the line will be trated as a comment.

pchar	Prints the integer stored in the top of the stack as its ascii value representation.

pstr	Prints the integers stored in the stack as their ascii value representation. It stops printing when the value is 0, when the stack is over and when the value of the element is a non-ascii value.

rotl	Rotates the top of the stack to the bottom of the stack.

rotr	Rotates the bottom of the stack to the top of the stack.

stack	This is the default behavior. Sets the format of the data into a stack (LIFO).

queue	Sets the format of the data into a queue (FIFO).
