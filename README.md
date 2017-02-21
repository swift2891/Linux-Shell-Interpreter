# Linux-Shell-Interpreter
Design Overview:
Geting Interactive Input:
	If a file path is provided as argument, the program wil go into Batch mode(executing batch commands), otherwise it will go into interactive mode. In interactive mode, the user is prompted for input and the input is stored in a character array using the scanf statement. The return value of scanf statement will be monitored for negative values, as Ctrl+D press can cause negative return. The array along with a pointer is passed as arguments to parse function.
	
How Parsing is done?
	The input string is iterated from beginning to end, during which the characters will be copied into the character pointer(q). Whenever it encounters spaces, program will nullify it and proceed. Whenever a semi-colon is detected, program will pause the parsing action and call the exec_cmd function to execute the complete command received so far. 
	
How Child process is handled?
	The fork() function is invoked for each successful command received. The Parent wont wait for the newly created child processes and proceeds with parsing the rest of the string. At the end of string, the parent will wait for all child created, by looping the wait function.  
	
Detailed Specifications:
Handling multiple semi-colon:-
	The program will get ready to call exec_cmd routine, if it sees a semicolon. But how does it know whether a possible command is parsed. Whenever a character [except " ",\n,\t and ;] is detected in the string, a flag(semi_good) is set. Based on that flag, the program knows, whether to call the exec_cmd func or not. Whenever a semicolon is detected, flag (semi) is set. So, when there are semi colon without having any proper cmd, it will be ignored. 
	
Handling space character:-
	Whenever a space character is detected, that particular location will be nullified('\0') and proceeded. If there are characters after space, they will be copied into the character pointer (q) and proceed till it hits a semicolon. There are two while loops, stacked at top and bottom, which iteratively removes all extra space characters.
	
No command b/w semicolon:
	As explained above, semi_good flag will tell the program, whether it's good to execute the exec_cmd routine.
	
Warnings:-
1. Invalid commands/command format will be detected by the return values from execvp function. Based on that warning will be provided. 
2. Extra Semicolon or Space will also be alerted to user, based on flags semi and semi_good. 
3. File not available or unable to open: fopen return values will be monitored for warnings.          
  
Minor Bug:
1. Sometimes, the terminal prompt while executing multiple commands in Batch file, will print at the middle and at the end of program, it appears hanged, but pressing Return key will bring the prompt back.   
