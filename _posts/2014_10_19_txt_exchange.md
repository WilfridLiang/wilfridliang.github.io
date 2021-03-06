# Windows和Linux下TXT排版转换

最近因为学校作业的关系，要求交代码。

我是习惯在Linux下打代码的，但这样的话，问题就来了，Linux下的源文件拿到Windows下打开的话，整个文件的排版就完全乱了。

我是要交给老师的，我也不知道老师是不是用的Linux系统。所以我打算写一个转换格式软件用来转换格式(其实也不算是转换格式，就是让排版可以正常显示)。

要做这样的一个软件，首先要明白Linux和Windows下TXT排版的区别。

它们的区别主要是行结束标志的区别，即EOL(end of line)，Windows下TXT是用"\r\n"两个字符来标记一行的结束的，而Linux则是用"\n"一个字符，没有"\r"。至于为什么有这种区别，就要追溯到打字机的年代了，这里不详细说明。

知道它们的区别之后，就可很简单的写出转换软件了。

Windows下的TXT文件把所有的"\r\n"中的"\r"删除，就可以在Linux下正常显示了。

Linux的TXT文件则是在所有的"\n"之前加入"\r"。

下面给出代码

	#include <stdio.h>

	int formatted_to_linux(char file_name[]);

	int formatted_to_windows(char file_name[]);

	int
	main(int argc, char **argv)
	{
		int select = 0;	//windows format to linux or contract.
		char file_name[20] = "\0";

		//Remark
		printf("To change the text file from windows format to linux format,"
			" select 0, from linux to windows, please select 1\n"
			"The result saves as Done.txt in current file.\n");
		printf("Pleast input the file name: ");
		scanf("%s", file_name);
		printf("Please select the format(0/1): ");
		scanf("%d", &select);
		//If it is not zero or one, it errors.
		if (select < 0 || select > 1){
			printf("Error!\n");
			return 0;
		}
		//Returns -1 which means error, than outputs error message.
		if ((select == 0) ?
		     formatted_to_linux(file_name) : formatted_to_windows(file_name)){
			printf("Error!\n");
		}
		else {
			printf("Done!\n");
		}

		return 0;
	}

	int
	formatted_to_linux(char file_name[]){
		char char_one = '0', char_two = '0';
		FILE *in_ptr = NULL, *out_ptr = NULL;

		//Open files, the result is in Done.txt.
		in_ptr = fopen(file_name, "rt");
		out_ptr = fopen("Done.txt", "wt");
		do{
			char_one = fgetc(in_ptr);	//Get a character
			//The character is not '\r', it goes to else statement to print it.
			if (char_one == '\r'){
				//Checks the next character, if it is '\n', prints a '\n' instead of "\r\n".
				char_two = fgetc(in_ptr);
				if (char_two == '\n'){
					fprintf(out_ptr, "%c", '\n');
				}
				else {	//If the next character is not '\n', prints both of them.
					fprintf(out_ptr, "%c%c", char_one, char_two);
				}
			}
			else {
				fprintf(out_ptr, "%c", char_one);
			}
		}while (char_one != EOF && char_two != EOF);
		//All have done, closes files.
		fclose(in_ptr);
		fclose(out_ptr);

		return 0;
	}

	int
	formatted_to_windows(char file_name[])
	{
		char char_one = '\0';
		FILE *in_ptr = NULL, *out_ptr = NULL;

		//Open files
		in_ptr = fopen(file_name, "rt");
		out_ptr = fopen("Done.txt", "wt");
		do {
			char_one = fgetc(in_ptr);	//Get a character.
			if (char_one == '\n'){	//If it is a '\n', puts a '\r' before it.
				fprintf(out_ptr, "%c%c", '\r', char_one);
			}
			else {	//If it isn't, prints it only.
				fprintf(out_ptr, "%c", char_one);
			}
		}while (char_one != EOF);
		//All have done, closes files.
		fclose(in_ptr);
		fclose(out_ptr);

		return 0;
	}

