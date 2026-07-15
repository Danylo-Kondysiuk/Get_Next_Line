
---

# get_next_line

*This project has been created as part of the 42 curriculum by dkondysi*

## Description

The goal of this project is to write a function that returns a line read from a file descriptor. Whether reading from a text file or from the standard input, the `get_next_line` function allows you to read the content one line at a time until the end of the file. This project introduces the concept of **static variables** in C programming and focuses on efficient memory management and buffer handling.

## Instructions

### Compilation

The function is designed to be compiled with the flag `-D BUFFER_SIZE=xx`, which defines the buffer size for the `read()` function.

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c

```

### Execution

## Main

```c
#include "get_next_line.h"
#include <stdio.h>
#include <fcntl.h>

int	main(int argc, char **argv)
{
	int		fd;
	char	*line;
	int		line_count;

	line_count = 1;
	// If a filename is provided as an argument, open it 
	if (argc > 1)
		fd = open(argv[1], O_RDONLY);
	else
		// Otherwise, read from Standard Input 
		fd = 0;
	
	if (fd == -1)
	{
		perror("Error opening file");
		return (1);
	}

	// Repeatedly call the function until it returns NULL [cite: 88, 91]
	while ((line = get_next_line(fd)) != NULL)
	{
		printf("Line [%d]: %s", line_count++, line);
		free(line); // All heap-allocated memory must be freed [cite: 19]
	}

	if (fd != 0)
		close(fd);
	return (0);
}
```

To use the function in your code, include the header:

```c
#include "get_next_line.h"

```

And call it in a loop to read a full file:

```c
char *line;
while ((line = get_next_line(fd)) != NULL)
{
    printf("%s", line);
    free(line);
}

```

## Algorithm Justification

The selected algorithm utilizes a **Static Variable** to maintain a persistent state between function calls. This is necessary because the `read()` function may pull more data into the buffer than what is needed for a single line (i.e., it might read past a `\n`).

**Steps of the Algorithm:**

1. **Reading:** The function reads from the file descriptor in chunks of `BUFFER_SIZE` and joins these chunks into the static string until a newline character (`\n`) is detected or EOF is reached.
2. **Extraction:** Once a newline is found, the function extracts the portion of the string from the beginning up to and including the `\n`.
3. **Updating:** The static variable is then updated to store only the "leftover" characters that follow the newline, ensuring they are available for the next time the function is called.
4. **Memory Management:** Each step involves careful allocation and freeing to ensure no memory leaks occur, especially when the end of the file is reached.

## Resources

* [Unix System Calls - Read](https://man7.org/linux/man-pages/man2/read.2.html)
* [Understanding Static Variables in C](https://www.geeksforgeeks.org/static-variables-in-c/)

### AI Usage

AI (Large Language Model) was utilized in this project for the following tasks:
* **Code Review:** AI was used to double-check that the function prototypes and file structure matched the requirements of the project subject.