## Part 1: Debugging Scenario

Below is a *simulation* of what an EdStem post on a debugging scenario would look like. Firstly, a student publishes a question on EdStem, after of course looking through similar posts and seeing if the question has already been answered. 

1. After confirming that there are apparently **no** related posts, the student posts the following question below:
<img width="597" alt="Lab Report 5 SC 2" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/7363b540-71b8-4ec5-b71f-9b519747a92c">

2. A TA graciously responds to the question that the student posts, by providing the following insight to the student:
<img width="596" alt="Lab Report 5 SC 3" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/b8e33fa1-e67f-456b-804c-988745991404">

3. By following the TA's instructions on how to resolve the bug, the student's code showcases the following output:
<img width="423" alt="Lab Report 5 SC 4" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/5b4a4a01-26c3-43c2-8577-1685a459b28c">

4. In the end, the student received feedback that helped them with identifying what was the **bug** that causes the **behavior** in their code. They were able to identify the following:

- The location of the working file and directory:
The student is currently working in the `list-examples-grader` directory, handling the files `GradeServer.java`, `Server.java`, `TestListExamples.java`, `ListExamples.java`, and `TestListExamples.java` in the terminal. 

- The contents of each of the relevant files **before** they fixed the bug:
The relevant file contents were the code in the `list-examples-grader` directory. In particular, the student pays particular attention to the code of `TestListExamples.java`:

```
import static org.junit.Assert.*;
import org.junit.*;
import java.util.Arrays;
import java.util.List;

class IsMoon implements StringChecker {
  public boolean checkString(String s) {
    return s.equalsIgnoreCase("moon");
  }
}

public class TestListExamples {
  @Test(timeout = 500)
  public void testMergeRightEnd() {
    List<String> left = Arrays.asList("a", "b", "c");
    List<String> right = Arrays.asList("a", "d");
    List<String> merged = ListExamples.merge(left, right);
    List<String> expected = Arrays.asList("a", "a", "b", "c", "d");
    assertEquals(expected, merged);
  }
}
```

- The set of necessary commands for them to run to induce the bug:
Running the `javac` command with the `-cp` command line option to specify that these are the files that need to be compiled. Seeing that the `Server.java` and `TestListExamples.java` file compiled, they now see that the issues lie in the `javac` command being run on the `ListExamples.java` file and `TestListExamples.java` file. 

The student sees that the issue does not particularly lie in actually testing `ListExamples.java`, in that the methods in the file work properly, but they see that the JUnit library is properly configured for runtime, **not** for compilation. The student realizes that the first part of the software development lifecycle is not the issue, the second part is.

- The steps needed to be taken to resolve the bug:
The student now realizes that for the purposes of the assignment, they are **NOT** instructed with trying to figure out how to compile this program, as it is not the problem that they are intended to solve. Seeing that their methods in `ListExamples.java` will be pass runtime testing, they now confirm that their files are properly configured.

The student realizes that the error-inducing input is their usage of the `javac -cp .:lib/junit.jar GradeServer.java Server.java TestListExamples.java ListExamples.java` command. 

They realize that their bug is the inconsistency in the runtime and compiler class paths. They now understand that for the purposes of the assignment, they're only to be tasked with ensuring the tests pass at **runtime**, not necessarily for compilation.

They also note the second bug of their files not being properly accessed; the student takes note that upon them trying to compile the program, the `ListExamples.java` file is not being properly accessed, therefore the `StringChecker` interface cannot be properly accessed for compilation.

In terms of a resolution, they simply ignore the problems that occur with compiling, and continue to run the tests of `TestListExamples.java` at runtime. 

By working cooperatively with the TA, and by giving themself some grace to sit down and work through the tedious process of resolving a coding bug, the student was now able to resolve their issue, which received an "answered" checkmark on EdStem.

## Part 2: Reflection

I think one of the most intriguing things I learned about towards the end of this class was the `vim` text editor. The `vim` command was very interesting to me not only because I was able to learn a nifty set of tricks and commands to navigate through my code much more efficiently, but also because what I found out about it beyond the scope of just the labs and lectures. 

I considered that because `vim` functions as a text editor, I would be able to obviously edit the text and composition of my code files. Additionally, I also saw that there are various plugins that I could use to further enhance my `vim` experience. Such plugins include `vim-taskwarrior`, which as the name implies, will allow you to become a much more proficient task manager. You can create a list of tasks additionaly with the options of adding dates, tags, dependencies, etc. Although I haven't really developed many skills in `vim` just yet, I do see myself using such a tool to help with planning any major projects, and furthermore executing them with an organized workflow. 

Another useful tool I found associated with `vim` was `Syntastic`. (I'm noticing the creators of these plugins have fun with the titles). It is a plugin that helps with identifying any potential issues, such as syntax bugs, in your code. It does so on a line-by-line basis, wherein as the user navigates through their files in `vim`, they will be confronted with an error message related to syntax in the command window. It will even display error pop-up messages over the line that contains the error when you hover your mouse over it. All in all, it can help simplify the process of identifying error-inducing inputs that commonly requires the use of `vim` to fix. By now knowing that these various plugins exist, I can see how the normally very daunting process of revising code can become a litle bit easier to tackle. 
