# Frida
## frida-trace
*frida-trace* is a tool provided by the Frida framework that allows you to dynamically trace and intercept function calls and other events in running processes. It is particularly useful for analyzing and reverse engineering applications to understand their behavior and interactions with various libraries and APIs.
### How to use it
1. Run a target program in terminal 1.
2. In terminal 2, trace some function calls, such as recv and read, with ``frida-trace -i "recv*" -i "read*" ShowCase``
3. (optional) Change the information to log, by editing the auto-genrated files for each function.
4. Observe how the frida process output information about the target program and the target functions.
### How it works
1. Instrumentation:
    When you run frida-trace on a target process, Frida injects its runtime agent into the process. This agent contains JavaScript code that establishes communication between Frida and the target process.

2. Module Loading:  
    Frida's agent code intercepts the loading of dynamic libraries (DLLs) and shared objects by the target process. This is typically done using functions like dlopen on Unix-like systems or similar functions on other platforms.

3. Function Tracing:  
    Once a library is loaded, Frida's agent identifies functions and symbols within that library. frida-trace allows you to specify functions to trace using patterns, regular expressions, or explicit function names. For each function you specify, Frida installs a hook that intercepts calls to that function.

4. Event Handling:  
    When the traced function is called, Frida's hook intercepts the call and allows you to execute custom JavaScript code before or after the original function call. This is known as a "hook point." You can access and manipulate parameters and return values, gather data, and even replace the original function's behavior with your custom logic.

5. Output:  
    As the traced process runs and calls the traced functions, frida-trace outputs information about each intercepted call. This includes details such as the process ID, module name, function name, and any relevant arguments and return values.

6. Dynamic Analysis:  
    By tracing function calls and examining their parameters and return values, you can gain insights into how the target process interacts with various libraries and APIs. This is particularly useful for reverse engineering, debugging, and understanding the internal workings of applications.