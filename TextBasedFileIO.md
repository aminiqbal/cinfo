# Basic TEXT File Input/ Output In C

* Text based file I/O in C used the ```stdio``` library. You must include this in your C program:
```
#include <stdio.h>
```

* C works with file streams.
* File streams are any source/ destination of input/ output that are able to be processed by the program.

---
* Essentially, C does this via creating a **POINTER** variable that *represents* the file stream.
	```
	FILE *file_stream = fopen("path/to/text/file.txt", "r");
	```

* We must also add a check such that if the provided path is invalid, the file does not exist or there was some other unexpected problem with opening a file stream for the specified file, our program will not crash unexpectedly, but will handle such an event in a stable way (perhaps by printing a message describing the error) and/ or terminate properly.
	```
	FILE *file_stream = fopen("path/to/text/file.txt", "r");

	if(file_stream == NULL)
	{
		printf("There was an error and the file could not be opened.\n");
		return 0;
	}
	```

	**OR**
	```
	FILE *file_stream;

	if((file_stream = fopen("path/to/text/file.txt", "r")) == NULL)
	{
		printf("There was an error and the file could not be opened.\n");
		return 0;
	}
	```

* As seen above, a file stream pointer is created by invoking the function ```fopen```:
	* The first parameter of fopen is a character array (string) which is the path to the file.
		* It is best to use the *absolute* path, as in the complete path to the file:

		```"C:/Data/text_file.txt"```

		* Note that if we are using a Windows operating system, we are able to use ```\``` instead of ```/```, but we must escape the ```\``` by placing another ```\``` in front of the initial one. This is because a single backslash is regarded by the compiler as a signifier that whatever comes after it will be an escape character as specified in documentation:

		```"C:\\Data\\text_file.txt"```

		* If we already know that the file will be located in our *source directory* (Path where our program is run from), then just providing the file name as a string will suffice:

		```"text_file.txt"```

	* The second parameter is another character array that specifies in what mode the file will be opened.
		* Note that switching a file stream from read mode to write mode requires a bit more work. Hence if we know we will be doing more operations on a file such as reading AND writing to it, we will have to use specific modes.
		* Read only is specified by ```"r"```. Be warned that unless a check is in place, if the specified file does not exist, or the path provided is an invalid one, then the program will simply crash.
		* Write only is specified by ```"w"```. Be warned that if our specified file already exists, then the file will be overwritten with content provided during our current program run. Otherwise, a new file will be created.
		* Append only is specified by ```"a"```. This will write to the end of the file if it exists, otherwise it will create the file.
		* Both read & write is specified by ```"r+"```. If the file does not exist, this will not work. If it does, writing to it will simply proceed to overwrite the file character by character. For example, if the file initially contained text ```abcde``` and we wrote to it ```mn```, then the file after our program execution would contain the text ```mncde```.
		* Both write & read is specified by ```"w+"```. This will overwrite the file if it exists, otherwise it will create the file. The file will also be able to be read from. Obviously, if the file does not exist initially, you cannot & should not attempt to read from it unless you have written to it first.
		* Both append & read is specified by ```"a+"```. This will write to the end of the file if it exists, otherwise it will create the file. The file will also be able to be read from. Obviously, if the file does not exist initially, you cannot & should not attempt to read from it unless you have written to it first.

---
## Text File Output

* Writing to a file is simple (We use the function ```fprintf```):
	* This works exactly like printf, but instead of writing to console, we are actually writing into a file.
	* Furthermore, the first parameter will have to be the file stream pointer and only then, followed by the usual formatted text including format specifiers and their respective variables/ values.
	* Each call of ```fprintf``` will continue appending to the **specified file stream** in order **as long as we are in that particular program instance**.
	```
	fprintf(file_stream, "The final value is %d.\n", value);
	```

* We are also able to write to files one character at a time, although this is slower and less frequently used (We use the function ```putc```):
	* This works similar to ```fprintf``` except that this function has 2 parameters:
		* 1st: Our desired character specified by **single quotes** or the character's respective ASCII integer value.
		* 2nd: The target file stream pointer.
	* **Note that for our purposes, at our current level, we will NOT use ```fputc``` as it is slower than ```putc```.**
	```
	putc('a', file_stream);
	```

---
## Text File Input

* There are various methods for reading from files.
	* We can read from a file *character by character* one character at a time and convert the character to our desired data type on the fly.
		* This method is best suited for reading files *we already know the format of*. For example, consistent CSV (Comma Separated Value) and other data files that contain numbers or characters that require meaningful interpretation.
	* We can read from a file *character by character* one character at a time.
		* This method is best used when we have to examine each character or when we know that our files will always contain a small amount of text. This is a bit more flexible than our aforementioned technique.
	* We can read from a file *one chunk at a time*.
		* This method is best for reading files containing a moderate to large amount of text. This method is significantly faster than just reading text character by character, since a whole block of text is being processed at a time.

* Reading a file character by character using ```fscanf```:
	* This is similar to how scanf works for taking in console input, except for the fact that just like ```fprintf```, the first parameter should be the file stream pointer.
	* Let us assume that our target file contains the name of a person followed by their age:
	```
	Jane
	25
	```

	We would be able to extract the name as a string and the age as an integer:
	```
	char name[10];
	int age;
	fscanf(file_stream, "%s\n%d", name, &age);
	printf("Name: %s\nAge: %d\n", name, age);
	```

	* We could also read the entire file character by character, although ```fscanf``` is not usually used for this purpose.
	```
	char c;
	while((fscanf(file_stream, "%c", &c)) != EOF)
	{
		printf("%c", c);
	}
	```

* Reading a file character by character using ```getc```:
	* **Note that for our purposes, at our current level, we will NOT use ```fgetc``` as it is slower than ```getc```.**
	```
	char c = getc(file_stream);
	while(c != EOF)
	{
		printf("%c", c);
		c = getc(file_stream);
	}
	```

	**OR**
	```
	char c;
	while((c = getc(file_stream)) != EOF)
	{
		printf("%c", c);
	}
	```

	* ```EOF``` is an integer value that indicates that the position (let us imagine the position to be an imaginary cursor with the length of 1 character) has reached the *end of file*. That is, there are no more characters in the file and the cursor/ character seeker has reached the end of the file. The proper value of ```EOF``` set by some reading function is -1.
	* Do **NOT** use the ```foef``` function or iterate through a file stream using ```while(!feof)```. The function ```feof``` does not equal the ```EOF``` generated by a reading function such as ```getc```. For an open file stream, ```feof``` will always return **FALSE** until some reading function has reached the end of the file and has triggered the ```EOF``` value to -1. The reason why it might *seem* to work is because if we program something like ```while((c = getc(file_stream)) != feof()){printf("%c", c);}``` is because upon reaching the end of the file, our ```getc``` function is triggering the value of ```EOF``` to -1 and returning the value, and since ```EOF``` has been set, ```feof``` is finally returning **TRUE** which is causing the loop to terminate. Therefore, at best, the ```feof``` function is *redundant* and most definitely should be avoided.

* Reading a file one chunk at a time using ```fgets```:
	* **Note that for our purposes, at our current level, we will NOT use ```gets```, as it is not 'limit safe'.**
	```
	int BUFFER_SIZE = 124;
	char buffer[BUFFER_SIZE];
	while((fgets(buffer, BUFFER_SIZE, file_stream)) != NULL)
	{
		printf("%s", buffer);
	}
	```

	* For extremely large text files, the buffer size can be increased, but usually a buffer size of 124 bytes is sufficient. Of course, processing speed is directly proportional to the buffer size, but it will obviously be inefficient if the buffer size exceeds the size of the total amount of text.

---
## Mandatory File Stream Close

* We must **ALWAYS** make sure that we have closed the file stream after we are done working with it (That is, either reading or writing from it, or both).
	* Otherwise this will lead to a *resource leak*.
	* It might also cause unexpected errors & program behavior.
	* The best practice is to close a file stream after ALL work pertaining to it with regard to the program is complete.
	* Sometimes, for big programs, it is best to open the file stream and close it once a certain modular task the requires the specified file is complete.
	```
	fclose(file_stream);
	```
