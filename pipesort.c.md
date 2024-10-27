### Documentation for C Code

This section of C code includes libraries, macro definitions, and sets up the basic error messages for a command-line program that likely deals with process creation and communication, possibly using pipes and sorting functionality. The following is a detailed breakdown of each part:

---

#### 1. **Included Libraries**

```c
#include <stdio.h> 
#include <stdlib.h>
#include <getopt.h>
#include <sys/types.h> 
#include <unistd.h> 
#include <sys/wait.h>
#include <string.h>
```

These `#include` directives import various libraries:

- **`<stdio.h>`**: Provides input/output functions, such as `printf`, `scanf`, `fgets`, and `fprintf`. This is commonly used for printing error messages or reading input.
  
- **`<stdlib.h>`**: Contains general-purpose functions such as memory allocation (`malloc`, `free`), program control (e.g., `exit`), and string conversion (e.g., `atoi`, `strtol`).
  
- **`<getopt.h>`**: Provides functions to parse command-line options (e.g., `getopt`), useful when handling options passed to the program, such as `-n`, `-s`, and `-l`.
  
- **`<sys/types.h>`**: Defines data types used in system calls, including data types for process IDs (e.g., `pid_t`), which are necessary for handling processes.

- **`<unistd.h>`**: Contains declarations for a variety of POSIX functions, including those for process control (`fork`, `exec`), pipes (`pipe`), and file operations (`read`, `write`). It’s essential for process management.

- **`<sys/wait.h>`**: Used for waiting on child processes and handling their termination (e.g., `wait`, `waitpid`). This is useful when processes created via `fork` are involved.

- **`<string.h>`**: Provides functions for manipulating C strings, such as `strlen`, `strcpy`, `strcmp`. These will likely be used for handling user input or command-line arguments.

---

#### 2. **Defined Macros (Error Messages)**

```c
#define ERR_USAGE "USAGE: pipesort -n sorter_cnt -s short -l long\n"
#define ERR_OPTION "pipesort: Invalid option: -%c\n"
#define ERR_INVALID_ARG "pipesort: Invalid option argument: -%c %s \n"
#define ERR_INVALID_ARG_CHAR "pipsort: expected -s < -l, but violates when -s=%d, -l=%d\n"
```

These macros define constant error messages, which the program can use to standardize its error reporting. These messages cover common issues such as invalid options or incorrect usage. The purpose of these macros is to simplify error handling by allowing the reuse of predefined error strings.

- **`ERR_USAGE`**: Informs the user about the correct usage of the program. It expects three command-line options: `-n`, `-s`, and `-l`. The user must specify the number of sorters (`sorter_cnt`), and two integer values (`short` and `long`). These will likely be used for controlling the behavior of the program, perhaps in terms of process creation and sorting limits.

- **`ERR_OPTION`**: A format string that indicates an invalid option was provided, displaying the erroneous option using the `%c` format specifier. For example, if the user provides an option that isn’t supported, this message will be printed with the invalid option character.

- **`ERR_INVALID_ARG`**: Indicates that an invalid argument was passed for an option. It takes two parameters, `%c` (the option character) and `%s` (the invalid argument itself). This could be useful for flagging incorrectly formatted inputs for `-n`, `-s`, or `-l`.

- **`ERR_INVALID_ARG_CHAR`**: Specifically handles the case when the value of `-s` (short) is expected to be less than the value of `-l` (long), but the condition is violated. This message displays the provided values of `-s` and `-l` using `%d` (integer format specifier).

---

#### 3. **Overview of Expected Program Functionality**

From the included libraries and macros, the program appears to be focused on:

- **Process Management**: The inclusion of `unistd.h`, `sys/types.h`, and `sys/wait.h` suggests that the program will be creating child processes (likely using `fork`) and managing inter-process communication (IPC), possibly via pipes.

- **Sorting**: The `pipesort` name suggests that the program implements some kind of sorting mechanism, potentially sorting data that is passed between processes using pipes. The `-n` option likely refers to the number of sorters (e.g., child processes), while `-s` and `-l` might define the range or limits of the data being sorted.

- **Command-line Interface**: The program heavily relies on command-line options, handled by `getopt`, which allows the user to control its behavior by providing arguments like `-n`, `-s`, and `-l`. Error messages are defined to ensure the user understands when incorrect options or arguments are provided.

---

### Potential Next Steps

As the documentation progresses, the following sections will require more detailed explanations:
- How the `getopt` function is used to parse command-line options.
- Description of the sorting functionality and the role of `pipes` for IPC.
- Explanation of how processes are created and managed using `fork`, `wait`, and `exec` functions.

Let me know if you want more specific parts of the code or functions documented!




















### Documentation for `is_alphabetic_do_lower` Function

This function checks if a given character is alphabetic (either lowercase or uppercase). If the character is uppercase, it converts it to lowercase. The function operates on a pointer to a character (`char * pc`) and returns `1` if the character is alphabetic (either lowercase or uppercase), and `0` otherwise.

---

#### **Function Prototype:**
```c
int is_alphabetic_do_lower(char * pc);
```

#### **Parameters:**

- **`char *pc`**: A pointer to a character that the function will evaluate and potentially modify. If the character is uppercase (A-Z), it will be converted to lowercase (a-z) by modifying the value pointed to by `pc`.

---

#### **Function Logic:**

1. **Check for Lowercase Characters**:
   ```c
   if (*pc >= 97 && *pc <= 122) {
       return 1;
   }
   ```
   - The function checks if the character is already lowercase (between ASCII values `97` ('a') and `122` ('z')).
   - If the character is lowercase, the function returns `1`, indicating the character is alphabetic, and no further action is taken.

2. **Check for Uppercase Characters and Convert to Lowercase**:
   ```c
   else if (*pc >= 65 && *pc <= 90) {
       *pc = *pc - 65 + 97;
       return 1;
   }
   ```
   - If the character is uppercase (between ASCII values `65` ('A') and `90` ('Z')), the function converts it to its corresponding lowercase counterpart by subtracting `65` (ASCII 'A') from the character and adding `97` (ASCII 'a').
   - After the conversion, the function returns `1`, indicating the character is alphabetic.

3. **Non-Alphabetic Characters**:
   ```c
   else {
       return 0;
   }
   ```
   - If the character is neither lowercase nor uppercase, the function returns `0`, indicating it is not an alphabetic character.

---

#### **Return Values:**

- **`1`**: If the character is alphabetic (either lowercase or uppercase). In the case of an uppercase letter, it is also converted to lowercase.
- **`0`**: If the character is not alphabetic.

---

#### **Example Usage:**
```c
char ch = 'A';
int result = is_alphabetic_do_lower(&ch);
printf("Result: %d, Modified char: %c\n", result, ch);  // Output: Result: 1, Modified char: a
```

- Input: `'A'` (uppercase letter)
- Output: `1` (indicating it's alphabetic), and the character is modified to `'a'`.

---

#### **Edge Cases:**

- If a non-alphabetic character such as a digit (`'1'`) or a special symbol (`'!'`) is passed, the function will return `0` without modifying the character.
  
  Example:
  ```c
  char ch = '1';
  int result = is_alphabetic_do_lower(&ch);
  printf("Result: %d, Modified char: %c\n", result, ch);  // Output: Result: 0, Modified char: 1
  ```

---

This function is useful for converting strings to lowercase while checking whether each character is alphabetic. Since it modifies the input character in place, you can use it for case-insensitive processing of alphabetic characters.














### Documentation for `do_parse` Function

The `do_parse` function reads characters from the standard input (`stdin`), processes them to form words composed of alphabetic characters, and distributes those words across a set of pipes for further processing. The function ensures that only words longer than a specified minimum length are written to the pipes, and that all alphabetic characters are converted to lowercase. Here's a detailed breakdown:

---

#### **Function Prototype:**
```c
int do_parse(int sorter_count, int max_char, int min_char, FILE **fps);
```

#### **Parameters:**

- **`int sorter_count`**: The number of pipes (or file pointers) where words will be written. This determines how many child processes or file streams are available for distributing the parsed words.
  
- **`int max_char`**: The maximum number of characters allowed in a word. This ensures that words longer than `max_char` are truncated.

- **`int min_char`**: The minimum number of characters a word must have to be written to the pipes. Words shorter than this threshold are ignored.

- **`FILE **fps`**: An array of file pointers (`FILE*`), one for each pipe or file. These pointers represent the output streams where the words will be written. Each word is written to one of these streams in a round-robin fashion.

---

#### **Function Logic:**

1. **Memory Allocation**:
   ```c
   char * word = (char *) malloc((max_char+1) * sizeof(char));
   ```
   - A buffer `word` is dynamically allocated to hold a word with up to `max_char` characters plus a null terminator (`+1`).

2. **Reading Input**:
   ```c
   while (fgets(buf, 2, stdin)) {
   ```
   - The function reads one character at a time from `stdin` using `fgets`. The buffer `buf[2]` is used to store the character and a null terminator.
   - `fgets(buf, 2, stdin)` reads 1 character per iteration and stores it in `buf[0]`.

3. **Handling Alphabetic Characters**:
   ```c
   if (is_alphabetic_do_lower(buf)) {
       if (char_cnt < max_char) {
           word[char_cnt++] = buf[0];
       }
   }
   ```
   - Each character is passed to the `is_alphabetic_do_lower` function, which checks if it's an alphabetic character and converts it to lowercase if necessary.
   - If the character is alphabetic and the current word is shorter than `max_char`, it is added to the `word` buffer, and the character count (`char_cnt`) is incremented.

4. **Non-Alphabetic Characters**:
   ```c
   else {
       word[char_cnt] = '\0';
       if (char_cnt > min_char) {
           fputs(word, fps[pipe_idx]);
           fputs("\n", fps[pipe_idx++]);
           pipe_idx %= sorter_count;
       }
       char_cnt = 0;
   }
   ```
   - When a non-alphabetic character is encountered (such as a space, punctuation, or newline), it signifies the end of a word.
   - The word is null-terminated (`word[char_cnt] = '\0'`), and if its length is greater than `min_char`, it is written to the current pipe (via `fputs`).
   - After writing the word, the `pipe_idx` (index of the pipe) is incremented and then wrapped around (`pipe_idx %= sorter_count`) to distribute words across multiple pipes in a round-robin fashion.
   - The character count (`char_cnt`) is reset to 0 to begin building a new word.

5. **End of Input**:
   ```c
   if (char_cnt > min_char) {
       fputs(word, fps[pipe_idx]);
       fputs("\n", fps[pipe_idx++]);
       pipe_idx %= sorter_count;
   }
   ```
   - When the end of input is reached, if there is an unfinished word in the buffer that meets the `min_char` requirement, it is written to the current pipe.

6. **Memory Deallocation**:
   ```c
   free(word);
   ```
   - The dynamically allocated memory for the `word` buffer is freed to prevent memory leaks.

7. **Error Handling**:
   ```c
   if (ferror(stdin)) {
       perror("Stdin read error");
       return 1;
   }
   ```
   - The function checks if there was any error while reading from `stdin`. If an error is detected, an appropriate error message is printed using `perror`, and the function returns `1` to signal an error.
   - If no errors occur, the function returns `0` to indicate successful completion.

---

#### **Return Values:**

- **`0`**: Successful execution with no errors.
- **`1`**: Error occurred while reading from `stdin`.

---

#### **Key Points:**

- **Word Parsing**: This function parses alphabetic words from the input stream. Words are constructed from alphabetic characters and converted to lowercase if necessary.
  
- **Word Length Control**: Words longer than `max_char` are truncated, while words shorter than `min_char` are ignored.
  
- **Round-Robin Distribution**: The parsed words are distributed across a number of pipes (`fps` array) in a round-robin fashion, ensuring even distribution of work among the available pipes.

- **Error Handling**: The function detects any issues with reading from `stdin` and returns an appropriate error code.

---

#### **Example Usage:**

Suppose you want to parse input from `stdin` and distribute valid words to a set of pipes:

```c
FILE *pipes[3];  // Assume 3 pipes are opened elsewhere
int result = do_parse(3, 10, 3, pipes);
if (result == 0) {
    printf("Parsing completed successfully!\n");
} else {
    printf("An error occurred during parsing.\n");
}
```

In this example:
- Words up to 10 characters are parsed.
- Words shorter than 3 characters are ignored.
- The parsed words are distributed across 3 pipes.












### Documentation for `clean_tail_break` Function

The `clean_tail_break` function removes a newline character (`'\n'`) from the end of a string if it exists. This is commonly used after reading input that might include a trailing newline (e.g., when reading lines from a file or `stdin` with functions like `fgets`). Here's a detailed breakdown of how it works:

---

#### **Function Prototype:**
```c
void clean_tail_break(char *word);
```

#### **Parameters:**

- **`char *word`**: A pointer to a null-terminated C string that the function will modify if the last character is a newline (`'\n'`).

---

#### **Function Logic:**

1. **Get the Length of the String**:
   ```c
   size_t len = strlen(word);
   ```
   - The function calculates the length of the string `word` using the `strlen` function. The length returned by `strlen` does **not** include the null terminator (`'\0'`), but it includes all other characters in the string.

2. **Check for a Trailing Newline**:
   ```c
   if (word[len-1] == '\n') {
   ```
   - The function checks if the last character in the string (i.e., `word[len-1]`) is a newline (`'\n'`). If the string ends with a newline, the function proceeds to the next step.

3. **Replace Newline with Null Terminator**:
   ```c
   word[len-1] = '\0';
   ```
   - If a newline is detected at the end of the string, it is replaced with the null terminator (`'\0'`), effectively removing the newline character. This ensures that the string is "cleaned" and ready for further processing without a trailing newline.

4. **Return Statement**:
   ```c
   return;
   ```
   - The function does not return any value (`void`), and simply exits after performing the necessary modification (if applicable).

---

#### **Usage Example:**

```c
char word[] = "Hello, World!\n";
clean_tail_break(word);
printf("%s", word);  // Output: "Hello, World!" (newline removed)
```

- Input: `"Hello, World!\n"`
- Output: `"Hello, World!"` (the trailing newline is removed).

---

#### **Edge Cases:**

1. **String Without a Newline**:
   If the input string does not have a trailing newline, the function does nothing and simply returns. For example:
   ```c
   char word[] = "NoNewline";
   clean_tail_break(word);
   printf("%s", word);  // Output: "NoNewline"
   ```
   The string remains unchanged.

2. **Empty String**:
   If an empty string is passed to the function, `strlen(word)` returns 0. The condition `word[len-1] == '\n'` becomes invalid because there's no valid `word[len-1]` (no characters in the string). The function effectively does nothing in this case.

---

#### **Key Points:**

- **Purpose**: The function is designed to clean up strings that may have a trailing newline after input operations like `fgets`.
- **Safety**: It checks the string’s length before attempting to access `word[len-1]` and only modifies the string if a newline is present at the end.
- **Common Use**: This is a utility function often used in text processing, especially when reading from files or standard input where newlines are included.

---

This function is useful when handling user input or reading lines from files, as newline characters can interfere with further string processing (e.g., comparison or concatenation).












### Documentation for `do_merge` Function

The `do_merge` function reads and merges sorted words from multiple file pointers (pipes) and outputs the merged result. The function counts the occurrences of each word, ensuring that words from different file streams are merged in lexicographical order, and writes the word along with its count to `stdout`. Here's a detailed explanation of its working:

---

#### **Function Prototype:**
```c
int do_merge(int sorter_count, int max_char, FILE **fps);
```

#### **Parameters:**

- **`int sorter_count`**: The number of file pointers (pipes) from which sorted words are being read. Each file pointer corresponds to a sorted input stream.
  
- **`int max_char`**: The maximum length of each word, ensuring that the function correctly handles words up to `max_char` characters long.
  
- **`FILE **fps`**: An array of file pointers from which the sorted words will be read. Each file pointer corresponds to an open stream of sorted words.

---

#### **Function Logic:**

1. **Initialization**:
   - **Dynamic Memory Allocation**:
     ```c
     char **word_cur = (char **) malloc(sorter_count * sizeof(char*));
     ```
     - A 2D array `word_cur` is allocated, where each row holds a word read from one of the `sorter_count` file streams.
     - `word_out` is a buffer that will hold the word with the current "smallest" lexicographical order.
   
   - **Initial Setup**:
     ```c
     strcpy(word_out, "{");
     ```
     - `word_out` is initialized to `{`, which is a character beyond `z` in the ASCII table, ensuring any valid word will be smaller than this initial value.

2. **Reading Initial Words**:
   ```c
   for (i = 0; i < sorter_count; i++) {
       if (fgets(word_cur[i], max_char+1, fps[i]) == NULL) {
           word_fp_end_flag |= (1 << i);
           strcpy(word_cur[i], "");
           continue;
       } else {
           clean_tail_break(word_cur[i]);
           if (strcmp(word_cur[i], word_out) < 0) strcpy(word_out, word_cur[i]);
       }
   }
   ```
   - The function reads the first word from each input stream (if available). If a stream ends, a flag (`word_fp_end_flag`) is set to mark the stream as finished, and an empty string (`""`) is assigned to `word_cur[i]`.
   - The smallest word among the first words is stored in `word_out`.

3. **Main Merging Loop**:
   ```c
   do {
       // Get word count
       for (i = 0; i < sorter_count; i++) {
           if ((word_fp_end_flag & (1 << i)) != 0) continue;  // word_cur[i] ended
           while (strcmp(word_cur[i], word_out) == 0) {
               word_cnt += 1;
               while ((ret_fgets=fgets(word_cur[i], max_char+1, fps[i])) != NULL) {
                    if (strcmp(word_cur[i], "\n") != 0) break;
               }
               if (ret_fgets == NULL) {
                   word_fp_end_flag |= (1 << i);
                   strcpy(word_cur[i], "");
                   break;
               } else {
                   clean_tail_break(word_cur[i]);
               }
           }
       }
       if (strcmp(word_out, "")) fprintf(stdout, "%-10lu%s\n", word_cnt, word_out);
   ```
   - The loop starts by comparing each word in `word_cur[i]` with `word_out` (the current smallest word). If they match, the count (`word_cnt`) is incremented, and the next word from that stream is fetched using `fgets`.
   - Words are repeatedly fetched from a stream as long as they match `word_out`. Once a new word is read, the loop stops fetching from that stream.
   - If a stream reaches the end, it is marked as finished by setting the appropriate bit in `word_fp_end_flag`.

4. **Writing the Result**:
   ```c
   if (strcmp(word_out, "")) fprintf(stdout, "%-10lu%s\n", word_cnt, word_out);
   ```
   - After counting all occurrences of `word_out`, it is printed to `stdout` along with its count (`word_cnt`), formatted to a width of 10 characters.

5. **Checking End Condition**:
   ```c
   merge_end_flag = 1;
   for (i = 0; i < sorter_count; i++) {
       if ((word_fp_end_flag & (1 << i)) == 0) merge_end_flag &= 0;
   }
   if (merge_end_flag == 1) break;
   ```
   - The loop checks if all streams have reached the end. If all streams have finished (i.e., all bits in `word_fp_end_flag` are set), the `merge_end_flag` is set, and the loop terminates.

6. **Finding the Next Smallest Word**:
   ```c
   word_cnt = 0;
   strcpy(word_out, "{");
   for (i = 0; i < sorter_count; i++) {
       if ((word_fp_end_flag & (1 << i)) != 0) continue;
       if (strcmp(word_cur[i], word_out) < 0) strcpy(word_out, word_cur[i]);
   }
   ```
   - After processing the current smallest word, the function finds the next smallest word across all streams and resets `word_cnt` for the new word.

7. **Memory Deallocation**:
   ```c
   for (i = 0; i<sorter_count; i++) free(word_cur[i]);
   free(word_cur);
   free(word_out);
   ```
   - At the end of the function, all dynamically allocated memory is freed to prevent memory leaks.

---

#### **Return Value:**
- **`0`**: Indicates successful execution.

---

#### **Key Points:**

- **Merging Process**: The function reads words from multiple sorted streams, compares them, and outputs the smallest word with its count.
  
- **Round-Robin Stream Handling**: The streams are handled in a round-robin fashion to ensure that words from all streams are merged in lexicographical order.
  
- **Efficient Memory Usage**: The function allocates memory dynamically for word storage, ensuring that it handles variable input sizes without overflow.

---

#### **Example Usage:**
Assuming you have multiple sorted word lists in files, this function can merge and count them:

```c
FILE *fps[3];  // Open 3 files with sorted word lists
int result = do_merge(3, 10, fps);
if (result == 0) {
    printf("Merge completed successfully.\n");
} else {
    printf("An error occurred.\n");
}
```

This function efficiently merges sorted word lists into a single sorted list with word counts.













### Documentation for `main` Function

The `main` function is responsible for orchestrating a multi-process sorting and merging system. It creates parser, sorter, and merger processes that communicate via pipes to sort and merge word lists. Here's a detailed explanation of its functionality:

---

#### **Function Prototype:**
```c
int main(int argc, char *argv[]);
```

#### **Parameters:**

- **`int argc`**: The number of command-line arguments passed to the program.
  
- **`char *argv[]`**: An array of strings containing the command-line arguments.

---

#### **Command-Line Arguments:**

The function expects three command-line options:

- **`-n <sorter_count>`**: Specifies the number of sorter processes.
- **`-s <min_char>`**: Specifies the minimum character length for words.
- **`-l <max_char>`**: Specifies the maximum character length for words.

The program will terminate with an error if any argument is invalid or missing.

---

#### **Key Variables:**

- **`pids[]`**: Array storing process IDs. It tracks the main process (`pids[0]`), parser process (`pids[1]`), and merger process (`pids[2]`).
  
- **`pid_sorters[]`**: Dynamic array that stores process IDs of the sorter processes.
  
- **`pipefd_parse[][]` and `pipefd_merge[][]`**: 2D arrays of file descriptors for pipes. These pipes facilitate communication between:
  - Parser and sorter processes (`pipefd_parse`).
  - Sorter and merger processes (`pipefd_merge`).

---

#### **Steps in the `main` Function:**

1. **Argument Parsing**:
   ```c
   while ((opt = getopt(argc, argv, "n:s:l:")) != -1) { ... }
   ```
   - The function uses `getopt` to parse command-line arguments for the number of sorters, minimum word length, and maximum word length.
   - If any argument is invalid or missing, an error message is printed, and the program exits.

2. **Argument Validation**:
   ```c
   if (sorter_count == -1 || min_char == -1 || max_char == -1 || min_char >= max_char) { ... }
   ```
   - The program checks whether all required arguments are provided and whether `min_char` is less than `max_char`. If any condition fails, the program terminates with an error.

3. **Pipe Creation**:
   ```c
   pipefd_parse = (int **) malloc(sorter_count * sizeof(int *));
   pipefd_merge = (int **) malloc(sorter_count * sizeof(int *));
   ```
   - The program dynamically allocates memory for pipes (`pipefd_parse` and `pipefd_merge`), which are used to transfer data between parser, sorter, and merger processes.
   - Each sorter has two pipes: one for communication with the parser (`pipefd_parse`) and one for communication with the merger (`pipefd_merge`).

4. **Forking Parser and Merger Processes**:
   ```c
   for (int pidx=1; pidx<3; pidx++) { ... }
   ```
   - The function forks two child processes:
     - **Parser process** (`pids[1]`): Reads data from a source (such as a file) and sends it to the sorters through `pipefd_parse`.
     - **Merger process** (`pids[2]`): Reads sorted data from the sorters through `pipefd_merge`, merges the sorted streams, and outputs the final result.
   
   - In each process:
     - The unnecessary pipe ends are closed to avoid deadlocks.
     - The `do_parse` function is called in the parser process to parse data.
     - The `do_merge` function is called in the merger process to merge sorted data.

5. **Forking Sorter Processes**:
   ```c
   for (i=0; i < sorter_count; i++) { ... }
   ```
   - The function forks a separate sorter process for each sorter (`pid_sorters[i]`).
   - Each sorter process:
     - Reads data from its corresponding pipe (`pipefd_parse[i]`), sorts it using the `sort` command (via `execl`), and writes the sorted data to another pipe (`pipefd_merge[i]`).
     - The pipes related to other sorters are closed in the current sorter to avoid conflicts.

6. **Closing Unused Pipe Ends in Main Process**:
   ```c
   for (i=0; i < sorter_count; i++) { ... }
   ```
   - The main process closes all pipe ends (both read and write) since they are only used by the child processes.

7. **Waiting for Child Processes**:
   ```c
   for (i=0; i < 1 + sorter_count; i++) { ... }
   ```
   - The main process waits for all child processes (parser, merger, and sorters) to complete.
   - If any child process exits with a non-zero status (indicating an error), the main process prints an error message and exits.

8. **Clean-Up**:
   ```c
   free(pid_sorters);
   for (i=0; i < sorter_count; i++) {
       free(pipefd_parse[i]);
       free(pipefd_merge[i]);
   }
   free(pipefd_parse);
   free(pipefd_merge);
   ```
   - The dynamically allocated memory for pipes and sorter process IDs is freed before the program exits.

---

#### **Key Points:**

- **Process Management**: The program forks multiple child processes (parser, sorters, and merger) and manages their communication using pipes.
  
- **Pipe Communication**: Pipes are used to transfer data between the parser, sorters, and merger in a way that maintains isolation between processes.
  
- **Error Handling**: The program carefully checks for errors when creating pipes and forking processes, and handles clean-up in case of failure.

---

#### **Example Command:**
```bash
./pipesort -n 4 -s 1 -l 10
```
This command runs the program with 4 sorter processes, handling words with a length between 1 and 10 characters.

### Functions Called:
- **`do_parse()`**: Handles parsing logic in the parser process (not shown here).
- **`do_merge()`**: Merges sorted data from the sorter processes.
- **`execl("/usr/bin/sort", "sort", (char*)NULL)`**: Executes the `sort` command in the sorter processes.



