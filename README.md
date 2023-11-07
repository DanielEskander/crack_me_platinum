### Confiant Technical Assessment 
https://crackme.s3.amazonaws.com/crack_me_platinum.html

My solution to this puzzle included many steps:

### 1. Displaying the Goal Message:
To begin reverse engineering the Javascript code, I decided to start by finding out the result of the crack me challenge. By diving into the code, it appeared that the end state must be inside the `setInterval()` function as this function is run on repeat every two seconds. Through further analysis, the function contained a conditional statement that would check for the correct conditions to then prompt an alert, indicating a completed solution. The alert message:

```javascript
ee(b("YWxlcn"+"QoIkNvbmdy"+"YXR1bGF0aW9uc"+"yBPaCAxMzM3I"+"E9uZSEgWW91IGh"+"hdmUgZm91bmQgbXkg"+"dHJlYXN1cmUhIik="));
```

Upon seeing the message, I noticed there was nothing significant about the concatenated strings; however, I recognized that the strings were base64, so I ran a simple command to check the message and was greeted with the congratulatory message:

```javascript
alert(atob("YWxlcn"+"QoIkNvbmdy"+"YXR1bGF0aW9uc"+"yBPaCAxMzM3I"+"E9uZSEgWW91IGh"+"hdmUgZm91bmQgbXkg"+"dHJlYXN1cmUhIik="));
```

This message translated to:
```
alert("Congratulations Oh 1337 One! You have found my treasure!")
```

The alert upon completing the task:


![image](https://github.com/DanielEskander/crack_me_platinum/assets/55925126/89ab4cf3-dc77-4455-87f9-01d76b226de9)


### 2. Downloading the HTML File
One of the first hurdles I faced was the fact that I was unable to alter any of the Javascript code due to the debuggger being lauched every 2 seconds, halting the code execution when detecting DevTools being used. So, before attempting to complete the puzzle, I downloaded the HTML file, so I could change the Javascript locally.

### 3. Disabling the Debugger 
One part of the logic in the `setInterval()` function was to stop the code execution if any DevTools were open, and because this function was run every 2 seconds, there was no way to use the DevTools to my advantage. To remedy this, I opted on fully removing the line which stopped the code:

```javascript
// ee('dLb'.replace('L', 'e') +'ug' + '14r'.replace('14', 'ge'));
```

### 4. Displaying Meaningful Coordinates
When moving the X around the screen, there were updated values for the `gps` value to indicate motion. However, these values were represented by base64 strings which would make it difficult to pinpoint exacly which coordinates the X was at. Thus, I changed the `IChing()` function to display x and y coordinates in integer format:

```javascript
document.getElementById("gps").innerHTML = y.toString() + "," + x.toString();
```

### 5. Finding and Using the Clue:
Now that I could use DevTools and the coordinates of the X, I could use the debugger to track certain variables. The most important variable to was `clue`. This variable, through my testing, always contained a string stating 'LAMBERT'. Though this clue seemed insignificant, it would prove useful. 

### 6. Putting the Pieces Together
I studied the `setInterval()` function for some more time to understand how the conditional would work. I found out that the conditional would make two checks:
- The length of runes array must be greater than 2.
- The sum of ASCII values in the clue must be equal to 469 (modified later, see below).
With both of these being met, the user would be greeted with the congratulatory message.

The runes array would be filled with data upon the X being placed at the right set of coordinates. I pieced together the fact that the coordinates could represent the ASCII values of each character in the string. The fact that the length of string was an odd number through me off as coordinates required an x and y value, but I started testing. To my surprise, the first set of characters and coordinates worked and my runes array had a new entry. I continued to match coordinates with the ASCII values until my runes array was greater than 2.

The ASCII values for the characters in LAMBERT:
- L: 76
- A: 65
- M: 77
- B: 66
- E: 69
- R: 82
- T: 84


Matching ASCII values to coordinates of X:
- Move "X" to (x: 76, y:65) for 'L' and 'A'
   ```javascript
    runes
    // Outputs: ['alpha']
   ```
- Move "X" to (x: 77, y:66) for 'M' and 'B'
   ```javascript
    runes
    // Outputs: ['alpha', 'beta']
   ```
- Move "X" to (x: 69, y:82) for 'E' and 'R'
   ```javascript
    runes
    // Outputs: ['alpha', 'beta', 'gamma']
   ```
- Move "X" to (x: 84, y:0) for 'T'
   ```javascript
    runes
    // Outputs: ['alpha', 'beta', 'gamma']
   ```

It seemed that the last set of coordinates weren't need as the runes array was at a length of 3 which is greater than 2. However, after doing all of this, it seemed that the congradulatory message didn't pop up. I made sure the runes array was greater than 2, which it was, and checked on the `tt` variable to see if it equaled 469. The `tt` variable was greater than 469 which caused the conditional to fail. I believe that the check of the ASCII sum wasn't fine tuned for the 'LAMBERT' string as adding all the values would give a value of 519 which is no equal to 469. 

Through this, I decided to change the conditional of the `tt` variable so that it would return true if the ASCII value is greater than or equal to 469:
`tt == 469;` to `tt >= 469;`
With this change, I was met with the congradulatory message:

![image](https://github.com/DanielEskander/crack_me_platinum/assets/55925126/5d9990b4-d445-4671-ac31-27bc40bc9d64)
  
