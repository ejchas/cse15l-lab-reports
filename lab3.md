### Part 1: Bug in Array List Example

From the lab in Week 4, I noticed that the array list implementation in `ArrayExamples.java` was very buggy, as two of the tester methods in `ArrayTests.java` failed upon running the program. These tester methods indicate if the `reverseInPlace()` and `reversed()` methods work properly, per their instructions provided from the lab. Before making any changes to the code, the failure-inducing inputs I identified were in the tester methods:

```
@Test
  public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);


    int[] input2 = {1, 2, 3, 4};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{4, 3, 2, 1}, input2);


    int[] input3 = {1, 2, 3};
    ArrayExamples.reverseInPlace(input3);
    assertArrayEquals(new int[]{3, 2, 1}, input3);


    int[] input4 = {};
    ArrayExamples.reverseInPlace(input4);
    assertArrayEquals(new int[]{}, input4);
  }
 @Test
  public void testReversed() {
    int[] input1 = {1 , 2 , 3, 4};
    assertArrayEquals(new int[]{4, 3, 2, 1}, ArrayExamples.reversed(input1));
  }

```

When the program is run, these methods result in a test failure for specific inputs, making the failure-inducing inputs the `assertArrayEquals()` statements found in each tester method. 

While this implementation is buggy, I could stil identify some inputs that did allow the tester methods to pass in their current state by only passing in an array that had elements `{ 0 }` for `testReversed()`, and either one-element arrays or symmetric elements for `testReverseInPlace()`. What I mean by the latter is that an array can have the elements in an order such as this: `{1, 2, 2, 1}` and this would not induce a tester method failure, since in its current state, the tester method doesn't check for edge cases. Here's what happens when I run both the failure-inducing input and non-failure-inducing input tester methods:

#### Failure-Inducing: 

```
  @Test
  public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);


    int[] input2 = {1, 2, 3, 4};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{4, 3, 2, 1}, input2);


    int[] input3 = {1, 2, 3};
    ArrayExamples.reverseInPlace(input3);
    assertArrayEquals(new int[]{3, 2, 1}, input3);


    int[] input4 = {};
    ArrayExamples.reverseInPlace(input4);
    assertArrayEquals(new int[]{}, input4);
  }
 @Test
  public void testReversed() {
    int[] input1 = {1 , 2 , 3, 4};
    assertArrayEquals(new int[]{4, 3, 2, 1}, ArrayExamples.reversed(input1));
  }

```

##### Symptom:

<img width="664" alt="Lab Report 3 SC 2" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/88628e25-3a8f-4368-9068-e2d892dc3f1c">

#### Non-Failure-Inducing:

```
public class ArrayTests {
	@Test 
	public void testReverseInPlace() {
    int[] input1 = { 1, 2, 2, 1 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 1, 2, 2, 1 }, input1);

    int[] input2 = { 1 };
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{ 1 }, input2);
	}


  @Test
  public void testReversed() {
    int[] input1 = { 0 };
    assertArrayEquals(new int[]{ 0 }, ArrayExamples.reversed(input1));
  }
}
```

##### Symptom:

<img width="628" alt="Lab Report 3 SC 1" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/1b6fd2fa-a20f-4cfe-bec2-039556f628cc">


#### Bug

From running the tests and contemplaing on how the methods are supposed to work, I identified the relevant changes needed so that the tester methods cover everything from edge cases to single-element array cases. For the method `reverseInPlace()`, the changes I made are displayed with comparison to the original code:

##### Original:
```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

##### Fixed:
```
 static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length / 2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
```

And here are the fixes I made for the method `reversed()`:

##### Original:
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```

##### Fixed:
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
```

The bug in the `reverseInPlace()` method was that the elements were not properly indexed through without no changes to the original array itself. Elements would be passed in while already being overwritten by the method which would return an array that wouldn't show an *exact* reversed copy of the array. 

What my changes accomplish are:
* By splicing the iterating for loop to only go through half of the array, the elements are effectively being switched around from their index `i` to their place relative to the end of the array (`arr[arr.length - i - 1]`)
* The `temp` variable handles the overwriting of the value of `arr[i]` so that the original value isn't lost during the swap

The bug in the `reversed()` method was that the array that was supposed to be returned during the swapping of elements was not `newArray`. This would incur the issue of the original array `arr` retrieving elements from the empty `newArray`, which resulted an incorrect set of reversed elements. 

What my changes accomplish are:
* The iterated elements of the array `arr` are assigned to `newArray` in a reverse order, again by taking the index of the original array element `i` and swapping it with its associate position at the end of the array (`arr[arr.length - i - 1]`)
* The array returned is `newArray`, which is the new array that is supposed to be returned as the original array in reverse order

### Part 2: Exploring the `grep` Command

The `grep` command is commonly used to to find a specific pattern of characters, and returning all the the lines that contain the character. 
An example of this is using the `grep` command to filter out a specific file extension like `.txt` or `.jpg` in a directory, and finding all files in that directory with that extension.

There are also many different command-line options that can be paired with the `grep` command to perform a plethora of tasks. Here is a list of examples of four options being used in the `technical/` directory from the Week 5 lab:

#### Option `-c`:
The `-c` option is used to print out the count of lines that match a pattern. If I were to `grep` with with the text `the` as my input, `technical/` as the directory to search in, and working in the `docsearch/` directory, I would simply get a very long list of every single `.txt` file in the directory. However, next to this long list of files would be the number of times the pattern of words appears in each individual `.txt` file. There are many, many files, so here are just some of those found in the output:

```
accen@Ethans-Laptop MINGW64 ~/OneDrive/Documents/GitHub/cse15l-lab-reports/docsearch (main)
$ grep -r -c "the" technical/

accen@Ethans-Laptop MINGW64 ~/OneDrive/Documents/GitHub/cse15l-lab-reports/docsearch (main)
$ technical/plos/pmed.0020249.txt:266
technical/plos/pmed.0020257.txt:22
technical/plos/pmed.0020258.txt:22
technical/plos/pmed.0020268.txt:33
technical/plos/pmed.0020272.txt:40
technical/plos/pmed.0020273.txt:25
technical/plos/pmed.0020274.txt:24
technical/plos/pmed.0020275.txt:29
technical/plos/pmed.0020278.txt:13
technical/plos/pmed.0020281.txt:20
```
I am now being shown how many times the word `the` appears in these `.txt` files. In `technical/plos/pmed.0020257.txt`, the word appears 22 times. This type of command option can determine how often a keyword appears in a list of files. It can aid in finding a general idea of the contents of the files, and what they're talking about.

Another use I found for the `-c` command from reasearching was that by pairing it with the `i` option, I can refine the search to be *insensitive* to letter cases. That way, more of the inputted keywords, regardless of capitalization, will appear in results:

```
$ grep -r -ci "the" technical/

accen@Ethans-Laptop MINGW64 ~/OneDrive/Documents/GitHub/cse15l-lab-reports/docsearch (main)
technical/plos/pmed.0020257.txt:27
technical/plos/pmed.0020258.txt:23
technical/plos/pmed.0020268.txt:35
technical/plos/pmed.0020272.txt:41
technical/plos/pmed.0020273.txt:26
technical/plos/pmed.0020274.txt:26
technical/plos/pmed.0020275.txt:31
technical/plos/pmed.0020278.txt:15
technical/plos/pmed.0020281.txt:21
```

This refined search can help with finding a greater frequency of keywords which can make reporting specific keywords in quantity more accurate. 

#### Option `-h`:
The `-h` option gives an output of matching lines for an inputted keyword, but does not display the file name specifically. Using `grep` with the `-r` option to search recurisvely in the directory `technical/` allows you to find every single instance of the keyword without including the file name. (Again these `.txt` files can be very long so here's a shortened version of the text that was displayed from the file `technical/biomed/1471-2156-2-1.txt`):

```
grep -r -h "the" technical/biomed/1471-2156-2-1.txt

        the death of retinal ganglion cells (RGCs) and their axons,
        and is characterized by atrophic excavation of the optic
        that other factors, such as genetic susceptibility to
        without high IOP and the benefit of lowering IOP in some of      
        these individuals further suggest multiple factors
        pressure-induced damage [ 6]. 
```

The keyword I inputted was `the`, and obviously many, many lines appeared as a result. What I found interesting about this usage of `-h` was that I could almost summarize the entire `.txt` file by just reading through the returned lines. Almost all of the text is cohesive, and will grant the user a general understanding of the meaning of the text. 

I found another interesting usage of `-h` by pairing it up with the syntax `technical/{government, biomed}/`. Like the previous instance, no file name will be displayed, but I did get quite a few lines of text when I inputted the keyword `territory`:

```
grep -r -h "territory" technical/{government,biomed}/
territory and who return to work in the United States regularly. 8
and territory to work with one another and with a broad spectrum of
territory of most relevance to clients and their
part of the legal landscape in every U.S. state and territory.
resources available in every state and territory. Objectives
territory. Staff are also able to assume special initiatives and
in the affected state or territory. In making the decision, the
create world-class justice communities in each state and territory
Dakota. These five states and one territory received notices of
communities for executive directors of state and territory-wide
California, requires each of his territory managers to present an
territory managers solicit feedback from customers on their
territory - a huge area bounded by the Ventura, Kern, San
territory.
new territory.
```

(I again shortened the actual output since there are many lines). What this command will do is find instances of the keyword `territory` in *both* the `government/` and `biomed/` subdirectories. This can be used in order to make lines much more readable when analyzing the contents of the two subdirectories. 

#### Option `-e`:

The `-e` option allows for searching of patterns within specific directories or files, and there can be multiple patterns you can search for on the same command line. When I use the keywords `climate` and `justice` in the `biomed/` subdirectory, paired with `-r` to search recursively, I get the following output:

```
$ grep -r -e "climate" -e "justice" technical/biomed/
technical/biomed/1471-2148-2-12.txt:          were then acclimated to the test environment via a
technical/biomed/1471-2148-2-17.txt:        column, it is used as an indicator of steppe climates [ 14
technical/biomed/1471-2156-3-10.txt:        climate, soil properties and silvicultural measures [ 21 ]
technical/biomed/1471-2202-3-11.txt:          maintained in a climate-controlled room on a 12 h
technical/biomed/1471-2229-2-8.txt:        acclimated under our laboratory conditions. We also
technical/biomed/1471-244X-3-5.txt:        this sunny climate to only be exposed to illumination
technical/biomed/1471-244X-3-5.txt:        The climate, especially during this time, is predominantly
technical/biomed/1471-244X-3-5.txt:        such as in communities of more extreme climate, new factors
technical/biomed/1471-2458-2-3.txt:            vibration, microclimate, electromagnetic fields, levels
technical/biomed/1472-6785-1-5.txt:        of current developments in ecology and climate research [ 3
technical/biomed/1472-6785-1-5.txt:        large-scale climate on the dynamics at all trophic levels
technical/biomed/1472-6785-1-5.txt:        climate on the dynamics at and among individual trophic
```

Here, in this shortened version of the output, I see very instance of the word climate, and justice, which just happens to be zero, within the subdirectory `biomed/`. Here, I can find the exact position of where the keywords lie.

Even though I can use the keyword `climate` with the option `-e` to find every instance of it in the desired directory or file, I notice that it appears to be shown even when it is a substring of something else, such as the word "acclimated". To ensure that only instances of just the word "climate" appear in the output, I can use the `-w` option with the `-e` option to ensure that only the exact pattern of `climate` will appear in the output. I'll also find another keyword that will likely appear more often, `science`:

```
$ grep -r -we "climate" -we "science" technical/biomed/
technical/biomed/1471-2105-1-1.txt:        methods of systems science need to be used to analyze,
technical/biomed/1471-213X-1-6.txt:        contribution to the body of science, and partly is a common
technical/biomed/1471-2148-2-15.txt:        philosophy, sociology, economics and political science
technical/biomed/1471-2156-3-10.txt:        climate, soil properties and silvicultural measures [ 21 ]
technical/biomed/1471-2164-4-26.txt:          http://www.blackwell-science.com/products/journals/suppmat/PBI/PBI006/PBI006sm.htm.
technical/biomed/1471-2202-3-11.txt:          maintained in a climate-controlled room on a 12 h
technical/biomed/1471-2229-2-9.txt:        formalisms used in engineering and computer science [ 24 ]
```

Now, I get a list of every single file that will **only** showcase instances of the exact string "climate" and "science". It can be useful in trying to figure out exact quantities of patterns appearing, without needing to worry about the case of substrings.

#### Option `-v`:

Finally, the `-v` option. Unlike all of the other options up to this point, which will search and return every instance of the pattern I input as a keyword, it will actually do the opposite. It instead returns every single line that does **not** include the keyword I input. In the context of the `technical/` directory which I used `-r` to recursively search through, I picked a file in the `biomed/` subdirectory and tested to see how many lines would **not** contain the keyword `the`. This was the output, shortened:

```
grep -r -v "the" technical/biomed/rr74.txt




        Introduction
        circulation in many mammals. NO has been proposed as a
        circulation, and previous studies using NOS inhibitors [ 1,
        2] suggested that inhibition of NO increases acute hypoxic
        pulmonary vasoconstriction. Chronic NOS inhibition did not
        lead to development of pulmonary hypertension [ 3],
        however, possibly because of a decrease in cardiac output.
        These discrepancies have been addressed in recent studies
        using mice that are deficient in NOS isoforms.
        vasculature is eNOS [ 4]. iNOS is expressed in airway
        lung nervous tissue [ 4, 5, 7, 8]. Thus, all three NOS
        isoforms could contribute to modulation of pulmonary
        vascular tone. Studies using knockout mice for each of
        NO is important in modulating basal pulmonary vascular
        pulmonary hypertension.
        Severe sustained hypoxia causes pulmonary hypertension
        in many animals. Upregulation of all three NOS isoforms
        following severe hypoxia has been reported in rats [ 4, 13,
        17, 18] suggesting an increase in lung eNOS and iNOS
        upregulated following hypoxia and that this may account for
        present study we exposed wild-type mice to severe hypobaric
        hypoxia from birth and measured NOS mRNA and protein
        immunohistochemistry.


        Materials and methods

          Mice
          We studied F1-generation SV129 (Taconic, Germantown,
          NY, USA) and C57BL/6 (Jackson Laboratories, Bar Harbor,
          ME, USA) mice at age 6 weeks.

```

To my surprise, many lines in this file did not include the word "the". I assumed that since "the" is such a common article word that not many lines would appear. I also noticed that some capitalized versions of "the" were still present in the output. In any case, this usage of the `-v` option will allow for extraction of any information in files that do not have to do with specific topics. 

I think that to effectively extract information from topics **not** pertaining to the inputted keyword, I would need to combine `-v` with other commands, such as `-e` and `-i` to ignore case. I reran the same command, with some modifications:

```
grep -r -vi -e  "the" -e "cardiac" -e "lung"  technical/biomed/rr74.txt




        Introduction
        circulation in many mammals. NO has been proposed as a
        circulation, and previous studies using NOS inhibitors [ 1,
        2] suggested that inhibition of NO increases acute hypoxic
        pulmonary vasoconstriction. Chronic NOS inhibition did not
        lead to development of pulmonary hypertension [ 3],
        using mice that are deficient in NOS isoforms.
        vasculature is eNOS [ 4]. iNOS is expressed in airway
        isoforms could contribute to modulation of pulmonary
        vascular tone. Studies using knockout mice for each of
        NO is important in modulating basal pulmonary vascular
        pulmonary hypertension.
        Severe sustained hypoxia causes pulmonary hypertension
        in many animals. Upregulation of all three NOS isoforms
        following severe hypoxia has been reported in rats [ 4, 13,
        upregulated following hypoxia and that this may account for
        present study we exposed wild-type mice to severe hypobaric
        hypoxia from birth and measured NOS mRNA and protein
        immunohistochemistry.
```

Again, shortened output. I noticed that no version of the word "the" appeared in here. Along with that, no instances of the keywords `cardiac` or `lung` were present. Combining these commands all together may induce some more fruitful outputs, in that in order to effectively use the `grep` command, some creativity may be necessary to get the desired output. If you want to skim through texts, using `-h` may help. If you want to extract information outside of the main idea of the text, use `-v`. For more refined searches, use `-w` and `-i`. All of the command line options shown provide resourceful ways of processing information.

### Citations:

For the second section of the lab report, I used Google first to find the different options I could use with the `grep` command, which led me to the url `https://www.geeksforgeeks.org/grep-command-in-unixlinux/`. I looked at the list of different command options, took note of how they worked per their descriptions, and picked which ones I thought would best demonstrate nuances in how to use the `grep` command in my limited understanding of how it works. I also referred back to the instructions from the Week 5 lab to ensure that the syntax I used for the `grep` command was correct. Other important links that helped me find relevant information were `https://docs.rackspace.com/docs/use-the-linux-grep-command`, `https://docs.rackspace.com/docs/use-the-linux-grep-command`, `https://unix.stackexchange.com/questions/126514/how-can-i-use-grep-to-search-multiple-unnested-directories`.
