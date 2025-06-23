# 42 Project - Get Next Line (GNL)

**Get Next Line** is a C project where the goal is to implement a function that reads a line from a file descriptor, one at a time, without knowing the size of the file in advance. It allows you to read the file incrementally, line by line, which is a common task when working with files in C.

The **bonus** part of the project extends the functionality to handle multiple file descriptors and ensures efficient memory usage.

## Features

- **Basic functionality**:
  - `get_next_line(int fd, char **line)`: Reads a line from the file descriptor `fd` and stores it in `line`.
  - Handles reading from large files incrementally, loading only the next line when requested.
  
- **Bonus features**:
  - **Multiple File Descriptors**: Supports reading from multiple file descriptors at once.
  - **Memory Efficiency**: Manages memory efficiently, freeing memory for each line after reading it.

## Installation

1. Clone the repository to your local machine:
   ```bash
   git clone https://github.com/forestbearx/get_next_line.git
   cd get_next_line
   ```

2. Compile the project using `make`:
   ```bash
   make
   ```

   This will generate the `get_next_line.o` object file that you can include in your project.

## Usage

To use `get_next_line` in your C program, include the header `get_next_line.h` and link with the corresponding `.o` file when compiling your program.

Example:

```c
#include "get_next_line.h"

int main() {
    int fd = open("myfile.txt", O_RDONLY);
    char *line;
    
    while (get_next_line(fd, &line) > 0) {
        printf("%s\n", line);
        free(line);  // Remember to free the line after using it!
    }

    close(fd);
    return 0;
}
```

To compile the above code, you can use the following command:

```bash
gcc -Wall -Wextra -Werror main.c get_next_line.o -o my_program
```

## Functionality

### get_next_line

```c
int get_next_line(int fd, char **line);
```

- **Parameters**:
  - `fd`: The file descriptor to read from (e.g., `STDIN_FILENO` for standard input, or a file descriptor returned by `open()`).
  - `line`: A pointer to a string that will hold the read line. This string will be allocated dynamically.

- **Return value**:
  - `1` if a line was successfully read.
  - `0` if the end of the file has been reached.
  - `-1` in case of an error.

**Example**:

```bash
$ echo "Hello\nWorld\n42" > myfile.txt
$ ./my_program
Hello
World
42
```

### Bonus: Multiple File Descriptors

The bonus part extends the functionality to allow reading from multiple file descriptors. You can use the same `get_next_line` function to read from any number of file descriptors without conflict.

```c
int fd1 = open("file1.txt", O_RDONLY);
int fd2 = open("file2.txt", O_RDONLY);
char *line;

while (get_next_line(fd1, &line) > 0) {
    printf("File1: %s\n", line);
    free(line);
}

while (get_next_line(fd2, &line) > 0) {
    printf("File2: %s\n", line);
    free(line);
}

close(fd1);
close(fd2);
```

This allows your program to read from multiple files or stdin at the same time.

## Testing

The project includes various test cases to ensure the functionality works as expected. You can run the test suite with the following command:

```bash
make test
```

## Contributing

Feel free to fork the repository and submit improvements. If you find any bugs or have suggestions, open an issue or submit a pull request.

## License

This project is open-source and licensed under the MIT License.
