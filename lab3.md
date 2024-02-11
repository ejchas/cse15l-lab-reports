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
