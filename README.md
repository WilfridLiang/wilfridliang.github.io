# Windows和Linux下TXT排版转换

最近因为学校作业的关系，要求交代码。

我是习惯在Linux下打代码的，但这样的话，问题就来了，Linux下的源文件拿到Windows下打开的话，整个文件的排版就完全乱了。

我是要交给老师的，我也不知道老师是不是用的Linux系统。所以我打算写一个转换格式软件用来转换格式(其实也不算是转换格式，就是让排版可以正常显示)。

要做这样的一个软件，首先要明白Linux和Windows下TXT排版的区别。

它们的区别主要是行结束标志的区别，即EOL(end of line)，Windows下TXT是用"\r\n"两个字符来标记一行的结束的，而Linux则是用"\n"一个字符，没有"\r"。至于为什么有这种区别，就要追溯到打字机的年代了，这里不详细说明。

知道它们的区别之后，就可以很简单的写出转换软件了。

Windows下的TXT文件把所有的"\r\n"中的"\r"删除，就可以在Linux下正常显示了。

Linux的TXT文件则是在所有的"\n"之前加入"\r"。

下面给出代码

[Qidian](http://www.qidian.com)

  int main(void){
      printf("Hello, World!\n");
      
      return 0;
  }
