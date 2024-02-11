### Part 1: Bug in Array List Example

From the lab in Week 4, I noticed that the array list implementation in `ArrayExamples.java` was very buggy, as two of the tester methods in `ArrayTests.java` failed upon running the program. These tester methods indicate if the `reverseInPlace()` and `reverse()` methods work properly, per their instructions provided from the lab. Before making any changes to the code, the failure-inducing inputs I identified were in the tester methods, where the methods they test appear like this before making any changes:

```
// Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```

When the program is run, these methods result in a test failure, making the failure-inducing inputs the `assertArrayEquals()` statements found in each tester method. Since this implementation is buggy, I identified an input that did work in its current state by only passing in an array that had a `{ 0 }` array for `testReversed()`, and either one-element arrays or symmetric elements for `testReverseInPlace()`. What I mean by the latter is that an array can have the elements in an order such as this: `{1, 2, 2, 1}` and this would not induce a tester method failure, since in its current state, the tester method doesn't check for edge cases. Here's what happens when I run both the failure-inducing input and non-failure-inducing input tester methods:

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

### Part 2: Exploring the `grep` Command

The `grep` command is commonly used to to find a specific pattern of characters, and returning all the the lines that contain the character. 
An example of this is using the `grep` command to filter out a specific file extension like `.txt` or `.jpg` in a directory, and finding all files in that directory with that extension.
