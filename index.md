**Part One**

A failure-inducing input for the buggy program: 
```
public void testReverseInPlace() {
    int[] input2 = {1,2,3};
    ArrayExamples.reverseInPlace(input2);
    assertArrayEquals(new int[]{3,2,1}, input1);
```

An input that doesn't induce a failure:
```
public void testReverseInPlace() {
    int[] input1 = { 7 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 7 }, input1);
```
The symptom: 
 ![Image](testfail1.png)

The bug:
```
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```

Fixed: 
```
  static void reverseInPlace(int[] arr) {
    for (int i = 0; i < arr.length / 2; i += 1) {
      int placeholder = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = placeholder;
    }
  }
```
In the original code, the reversed values are not saved for the next iteration. This causes the final array to repeat numbers because certain values in the array are not updated correctly. In my code, we only iterate up to ```arr.length / 2``` to avoid the issue in the original code. I then store ```arr[i]``` in the temporary placeholder, which will later be set to ```arr[arr.length - i - 1]``` to "reverse" the array.

**Part two**
1. grep -r "example" .
-  ```grep -r "transliteration" ./technical```
-  Outputs:
- chapter-2.txt           we use its transliteration, e.g.,"al Qida" instead of al Qaeda.
- chapter-13.5.text             40. Among the more important problems to address is that of varying transliterations
-  ```grep -r "Nostalgia" ./technical```
-  Outputs:
- chapter-2.txt             Nostalgia for Islam's past glory remains a powerful force.
This command is useful if you want to search for a specific word, and when there are hundreds of files to look through. 

2. grep -v "example" file.txt
- ```grep -v "the" chapter-1.txt```
- Output (Omitted some of the output because too long): 
-     FAA: Yes.

    NEADS: On its way towards Washington?


    NEADS: Okay.



    NEADS: He-American 11 is a hijack?

    FAA: Yes.

    NEADS: And he's heading into Washington?

    FAA: Yes. This could be a third aircraft.
- ```grep -v "a" chapter-1.txt```
- Output (Omitted some of the output because too long): 
-     Center: Do you know who he is?
This command is useful if you want to search for lines in a specific line that don't contain a specific word. 


3. grep -e "pattern1" -e "pattern2" file.txt
