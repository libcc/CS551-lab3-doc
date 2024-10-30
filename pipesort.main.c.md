
### 1. **Main Function Declaration and Variables**
```c
int main(int argc, char *argv[]) 
```
- **argc** and **argv[]** are used to handle command-line arguments.
- The `extern` declarations for `optarg` and `optind` are related to parsing options from the command line using `getopt`.
  
### 2. **Command-line Arguments Parsing**
```c
while ((opt = getopt(argc, argv, "n:s:l:")) != -1) {
```
- The program expects three options:
  - `-n`: The number of sorter processes (`sorter_count`).
  - `-s`: The minimum character value (`min_char`).
  - `-l`: The maximum character value (`max_char`).
- For each argument, the value is parsed with `strtol()`. If a value is invalid, the program prints an error message and exits.

### 3. **Validation of Arguments**
```c
if (sorter_count == -1 || min_char == -1 || max_char == -1) {
```
- This checks if all required arguments have been provided and ensures that `min_char` is less than `max_char`. If not, it exits with an error.

### 4. **Process IDs and Pipes**
```c
int pids[3]; // 0: main, 1: parse, 2: merge
int * pid_sorters; // to store pids of sorter processes
```
- The array `pids` holds process IDs of the main process, parser process, and merge process.
- Pipes (`pipefd_parse` and `pipefd_merge`) are created for inter-process communication between parser-sorter and sorter-merger.

### 5. **Creating Pipes**
```c
for (i = 0; i < sorter_count; i++) {
    pipefd_parse[i] = (int *) malloc(2 * sizeof(int));
    if (pipe(pipefd_parse[i]) == -1) {
        perror("Pipe between parser and sorter");
        ret = 1;
        break;
    }
    pipefd_merge[i] = (int *) malloc(2 * sizeof(int));
    if (pipe(pipefd_merge[i]) == -1) {
        perror("Pipe between merger and sorter");
        ret = 1;
        break;
    }
}
```
- Pipes are created using `pipe()`. Each pipe consists of a read-end and a write-end (thus the allocation of two integers).
- `pipefd_parse` is used for communication between the parser and the sorters.
- `pipefd_merge` is for communication between the sorters and the merger.

### 6. **Forking Parser and Merger Processes**
```c
for (int pidx = 1; pidx < 3; pidx++) {
    if ((pids[pidx] = fork()) == -1) {
        perror("Fork parse and merge process");
        exit(1);
    }
    if (pidx == 1 && pids[1] == 0) {   // inside Parse Process
```
- Two child processes are forked:
  - **Parser Process** (`pids[1]`): Reads data, sends it to sorters via pipes.
  - **Merger Process** (`pids[2]`): Collects sorted data from sorters and merges it.
  
### 7. **Parser Process**
```c
for (i = 0; i < sorter_count; i++) {
    close(pipefd_parse[i][0]);   // close read end for parser
    fps[i] = fdopen(pipefd_parse[i][1], "w");  // write-end of pipe
}
parser_ret = do_parse(sorter_count, max_char, min_char, fps);
```
- The parser writes data to the sorters through the pipes (write-end).
- `do_parse()` sends the parsed data to the sorter processes.

### 8. **Merger Process**
```c
for (i = 0; i < sorter_count; i++) {
    fps[i] = fdopen(pipefd_merge[i][0], "r");  // read-end of pipe for merger
}
merger_ret = do_merge(sorter_count, max_char, fps);
```
- The merger collects the sorted data from the sorters (read-end).
- `do_merge()` merges the sorted data and outputs the final result.

### 9. **Forking Sorter Processes**
```c
for (i = 0; i < sorter_count; i++) {
    if ((pid_sorters[i] = fork()) == -1) {
        perror("Fork sorter process");
        exit(1);
    }
    if (pid_sorters[i] == 0) {   // This is sorter process
        dup2(pipefd_parse[i][0], STDIN_FILENO);
        dup2(pipefd_merge[i][1], STDOUT_FILENO);
        execl("/usr/bin/sort", "sort", (char*)NULL);
    }
}
```
- The main process forks `sorter_count` sorter processes.
- Each sorter reads data from `pipefd_parse[i][0]` (input) and writes the sorted result to `pipefd_merge[i][1]` (output).
- The `execl()` call replaces the sorter process with the Unix `sort` command.

### 10. **Waiting for Child Processes**
```c
for (i = 0; i < 1 + sorter_count; i++) {
    if ((whom = wait(&status)) == -1) {
        perror("Main wait");
        ret = 1;
        break;
    }
}
```
- The main process waits for all child processes (parser, merger, sorters) to finish using `wait()`.

### 11. **Clean Up**
```c
free(pid_sorters);
for (i = 0; i < sorter_count; i++) {
    free(pipefd_parse[i]);
    free(pipefd_merge[i]);
}
```
- The dynamically allocated memory for pipes and sorter PIDs is freed before the program exits.

### Summary:
- This code parses input arguments to determine the number of sorters and character range.
- It creates pipes to communicate between a parser process, multiple sorter processes, and a merger process.
- The parser sends data to the sorters, each sorter sorts its portion using the Unix `sort` command, and the merger collects and merges the sorted output.
