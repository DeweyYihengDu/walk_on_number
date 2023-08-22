Walk on Number
================
### How to use the codes:
What you should know:
+ When type the numbers, the codes only support the number under Base 10 (included).
+ Don't use the letters to show the numbers (such as 'b' shows '11', except question 4).
How to use the codes: ![figure](./Pasted%20_mage_20230504225513.png)


You can use terminal to use the codes. The codes support the `__main__` module. You just need to type the number to check every function, especially, type `7` can run the `test` function and type `0` to quit the running function. 
The answer to every question:
1. `find_period(num_tuple, base)[2]` is the answer to question 1
2. `find_nsteps(num_tuple, base, nsteps)[1]` is the answer to question 2
3. `from_prefix_period2mn(prefix,period,base)` is the answer to question 4
4. The answers to question 3 and 5 can used the test function to check manually.![[Pasted _mage_20230504225513.png]]
### Part 1: Returns the period of the expansion sequence.
+ **The function extracts numerator and denominator, calculates quotient and remainder using a loop, and returns the length of the period, the periodic sequence, the list of periodic digits, and the list of prefix digits.**
+ **If there is no period, the length of the period, the periodic sequence, and the list of periodic digits are set to zero.**
+ **To calculate the repeating decimal, the function identifies a loop by finding the same remainder when dividing by the successive quotient.**
+ **The quotient between the first and second appearance of the remainder indicates the period of the loop.**
+ **The number before the first appearance of the remainder represents the prefix.**
+ **When the decimal is finite, its period is forced to be empty.**


The function extracts the numerator and denominator, and then uses a loop to calculate the quotient and remainder, adding them to their respective lists. After the loop is finished, the function calculates the periodic sequence and the length of the period using the numbers in the lists, and returns the length of the period, the periodic sequence, the list of periodic digits, and the list of prefix digits. If there is no period, the length of the period, the periodic sequence, and the list of periodic digits are set to zero.

$$\begin{equation}
\begin{split}
\frac{numerator}{denominator}&= quotient_0 \times denomindinator + remainder_0\\
& \vdots \\
\frac{quotient \times Base}{demominator}&=quotient_i \times denomindinator + remainder_i
\end{split}
\end{equation}$$

When calculating the repeating decimal, assume that for (m, n), if the same remainder appears when dividing by the successive quotient, it means that a loop will occur. The quotient between the first and second appearance of the remainder indicates the period of the loop. The number before the first appearance of the remainder represents the prefix. **Note that when the decimal is finite, I force its period to be empty.**

### Part 2: Find the n_steps.

1. calculate directly. 

The function performs division directly and performs the operation nsteps times. This is suitable for cases with a small number of steps. The function extracts the numerator and denominator from the input tuple, and then uses a loop to calculate the quotient and remainder, adding them to a list. Before calculating each quotient, the function also calculates the minimum width of the number and uses the `str.zfill` method to ensure that each number has the same width. Finally, the function returns the generated number sequence in three formats: string format, integer list format, and string list format.

2. use prefix and period to calculate.

If nsteps is large, in order to improve efficiency, the function first calculates the prefix and period of (m, n), and then constructs nsteps using the repetition of the prefix and period according to the size of nsteps. The function extracts the numerator and denominator from the input tuple and calls the `find_period` function to calculate the period and prefix of the fraction. Then, based on the input number of steps and the length of the period, the function generates the prefix and period parts of the number sequence. If the input number of steps is less than or equal to the sum of the length of the period and the length of the prefix, the generated number sequence is the first few digits of the prefix plus the period part. If the input number of steps is greater than the sum of the length of the period and the length of the prefix, the generated number sequence is the prefix plus the period part, repeating the period part until the desired number of steps is reached, and then the last few digits are truncated. Finally, the function returns the generated number sequence in both string and list formats.

### Part 3: Draw the plot

In accordance with the case of direct calculation, the function first calculates the sequence of (m, n) in 4 base, and then creates a dictionary to record the coordinates of each point, and determines the change of coordinates through the if function. The key represents the order of the point. Finally, the function plots the graph based on the coordinates and key values.

The function extracts the numerator and denominator from the input tuple and calls the `find_nsteps` function to generate the number sequence. Then, based on the content of the number sequence, the function updates the coordinates of the path and saves each position and its corresponding counter value to a dictionary. Then, the function extracts the x and y coordinates from the dictionary and generates a list from 1 to the length of the number sequence. Finally, the function uses the `matplotlib` library to generate a scatter plot, with coordinates as the axis and counter as the colour, clearly showing the path of the number.

### Part 4: Return (m,n) from prefix and period

The function normalises the prefix and period and performs some transformations. First, it checks if the period is empty, and if so, sets the period to 0. If the prefix is 0, the function sets the prefix to the period.

$$ 
\begin{equation}
\begin{split}
prefix&.period...... (1)\\
prefix \ \ period&.period......(2)
\end{split}
\end{equation}
$$
The function eliminates the infinite repeating decimals at the end for convenience in calculation.
$$
\begin{equation}
\begin{split}
(1)&=\frac{m}{n} \times Base^{len(prefix)}\\
(2)&=\frac{m}{n} \times Base^{len(prefix)+len(period)}\\
(2)-(1)&=prefix \times Base^{len(prefix)}+period -prefix\\
&= \frac{m}{n} \times(Base^{len(prefix)+len(period)}-Base^{len(prefix)})\\
\frac{m}{n}&=\frac{prefix \times Base^{len(prefix)}+period -prefix}{Base^{len(prefix)+len(period)}-Base^{len(prefix)}}
\end{split}
\end{equation}$$
Then, it sets m and n as the numerator and denominator respectively, and then removes the greatest common factor to obtain the required m and n.
$$
\begin{equation}
\begin{split}
m&=prefix \times Base^{len(prefix)}+period -prefix\\
n&=Base^{len(prefix)+len(period)}-Base^{len(prefix)}
\end{split}
\end{equation}
$$
Finally, the greatest common divisor of m and n is deleted using the `gcd` function in `math`.

### Part 5: from plot to (m, n) and redraw the plot

+ `plot2point(plot)`: Input image, convert image to coordinates of points, label coordinates of non-white pixel points. 
+ `distance(point1, point2)`:Calculate the square of distance between 2 points.
+ `nearest_neighbor(points)`:Rearrange the coordinates of these points to the nearest distance.
+ `shortest_path(start, end)`:Find the shortest path between two points to get path of discontinuous points. Then, written in the format of a sequence, and the direction they travel is obtained by subtracting that point from the next, thus giving a sequence of paths in 4 decimal degrees.
+ Use the **part 4** to re-draw the plot. Use this sequence as a prefix for the drawing, with the length partially adjusted according to the length of the prefix (generally one prefix length, but depending on some accuracy issues in image to sequence, the length may need to be reduced slightly)
+ Use the path to re-draw the plot.
+ Hint: the number of every plot is in the text function.
### The main and test function
Use the main function to avoid some errors.
+ Incorrect formatting of input
+ M>N 
+ The image is not in the location

Use the `assert` to test some functions: `find_period`, `find_nsteps`, `from_prefix_period2mn`

### Reference

The article does not refer to any reference materials.