# Overview
The learning objective of this lab is understand how environment variables affect program and system behaviors. In this lab, we will understand how environment variables work, how they are propagated from parent process to child, and how they affect system/program behaviors. We are particularly interested in how environment variables affect the behavior of Set-UID programs, which are usually privileged programs. This lab covers the following topics:
+ Environment variables
+ Set-UID programs
+ Securely invoke external programs
+ Capability leaking
+ Dynamic loader/linker

---

## Lab Tasks

### Task 1: Manipulating Environment Variables
+ Use printenv or env command to print out the environment variables.
		![[Use printenv or env.png]]
	Note: both `printenv` and `env` are commands used to **display environment variables**, but they have slightly different behaviors and use cases:
	+ `printenv` : Prints the values of environment variables.
	+ `env` : Runs a command in a modified environment or displays the current environment if no command is given.
+ Use export and unset to set or unset environment variables.
		![[Use export and unset.png]]
	Note: It should be noted that these two commands are not separate programs; they are two of the Bash’s internal commands as such you will not be able to find them outside of Bash.	
### Task 2: Passing Environment Variables from Parent Process to Child Process
myprintenv.c : 
	![[myprintenv.png]]
+ Compile and run the program (myprintenv.c)
		![[compile and run myprintenv.png]]
	Note: myprintenv.c can be compiled using `gcc myprintenv.c`, which will generate a binary called a.out and can run and save the output into a file using `a.out > file`.
+ Modify myprintenv.c and Compile and run the code again
		![[modefied myprintenv.png]]
		![[compile and run modefied myprintenv.png]]
+ Compare the difference of these two files 
		![[file difference.png]]
	Note: We can conclude that the environment variables at the parent and the child processes are the same, which means that the child process inherits its environment variables from the parent process at the moment it is created with the call to fork().
### Task 3: Environment Variables and `execve()`
myenv.c : 
	![[myenv.c.png]]
+ Compile and run the following program (myenv.c)
		![[compile and run myenv.c.png]]
+ Change the invocation of `execve()`
		![[edit myenv.c.png]]
+ Draw your conclusion
		![[running edited myenv.c.png]]
	Note: The new program must get its environment variables explicitly through the `execve` call. As we saw from the task, if no environment variables are passed through the call, the program will not have access to them.
### Task 4: Environment Variables and `system()`
+ created system_call.c
	![[system_call.c.png]]
+ Running system_call.c
	![[run system_call.c.png]]
	Note: By using the system() call, the environment variables are passed to the program because it uses execl internally, which provides the environment variables to execve automatically.
### Task 5: Environment Variable and `Set-UID` Programs
+ created print_env.c
	![[print_env.c.png]]
+ Compile the above program, change its ownership to root, and make it a Set-UID program.
	![[Compile,change owner,set-UID print_env.c.png]]
+ set the following environment variables and run the Set-UID forking the EVs
	+ PATH
	+ LD LIBRARY PATH
	+ ANY NAME
	![[Set-UID Root Shell.png]]
	Note: The environment variables are passed to the Set-UID child process. Root Shell acquired from our set-UID program running PATH environment variable.
### Task 6: The PATH Environment Variable and `Set-UID` Programs
