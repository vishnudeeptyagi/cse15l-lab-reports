# LAB 3

## Part 1 - Bugs

A failure-inducing input for the buggy program, as a JUnit test and any associated code:
```
public class ArrayExamplesTest {
    @Test
    public void testReversedMethodFailure() {
        int[] input = {1, 2, 3, 4, 5};
        int[] expected = {5, 4, 3, 2, 1}; // Expected reversed output
        int[] result = ArrayExamples.reversed(input);
        assertArrayEquals(expected, result);
    }
}
```

An input that doesn’t induce a failure, as a JUnit test and any associated code:
```
public class ArrayExamplesTest {
    @Test
    public void testReversedMethodSuccess() {
        int[] input = {7, 10, 3, 8};
        int[] expected = {8, 3, 10, 7}; // Expected reversed output
        int[] result = ArrayExamples.reversed(input);=
        assertArrayEquals(expected, result);
    }
}
```

The symptom, as the output of running the tests (provide it as a screenshot of running JUnit with at least the two inputs above):
![ss](ss)

The bug, as the before-and-after code change required to fix it:
Before:
```
// Bug in reverseInPlace method
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
        arr[i] = arr[arr.length - i - 1]; // Incorrect assignment, it overwrites elements incorrectly
    }
}

// Bug in reversed method
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
        arr[i] = newArray[arr.length - i - 1]; // Incorrect assignment, it doesn't populate newArray correctly
    }
    return arr;
}
```

After:
```
// Fix for reverseInPlace method
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length / 2; i++) {
        int temp = arr[i];
        arr[i] = arr[arr.length - i - 1];
        arr[arr.length - i - 1] = temp;
    }
}

// Fix for reversed method
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i++) {
        newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
}
```
Why does the fix addresses the issue?
The fix addresses the issues by correcting the assignments in both reverseInPlace and reversed methods. In reverseInPlace, a swapping technique is used to properly reverse the array elements. In reversed, the fixed implementation correctly populates the newArray with reversed elements from the input array, ensuring the correct reversed output.


## Part 2 - Researching Commands

grep command

Option 1: -E (extended regular expression)
This option enables extended regular expressions for pattern matching.
Example 1:
Search for lines containing either "error" or "warning": ```grep -E "error|warning" ./technical/logfile.txt```
Example 2:
Search for lines that start with "success": ```grep -E "^success" ./technical/logfile.txt```

Option 2: -o (only matching)
This option displays only the matched parts of the lines.
Example 1:
Display only the email addresses in a file: ```grep -oE '[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}' ./technical/emailfile.txt```
Example 2:
Display only the filenames from a directory listing: ```ls -l ./technical/ | grep -oE '[^ ]+$'```

Option 3: -c (count)
This option displays the count of matched lines.
Example 1:
Count occurrences of "error" in a file: ```grep -c "error" ./technical/logfile.txt```
Example 2:
Count lines starting with "warning": ```grep -c "^warning" ./technical/logfile.txt```

Option 4: -B (before context lines)
This option displays lines before the matched line.
Example 1:
Display two lines before lines containing "error": ```grep -B 2 "error" ./technical/logfile.txt```
Example 2:
Display one line before lines starting with "warning": ```grep -B 1 "^warning" ./technical/logfile.txt```


