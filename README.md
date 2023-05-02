Download Link: https://assignmentchef.com/product/solved-coms228-assignment2-coordinate-points-and-vectors
<br>
In computational geometry, algorithms often build their efficiency on processing geometric objects in certain orders that are generated via sorting. One example is a <em>Graham scan</em>, which constructs the convex hull of a collection of points in the plane in one traversal ordered by the polar angle. In another example, intersections of a set of line segments can be found using a sweep line that makes stops only at the segments’ endpoints and intersections. ordered by <em>x</em>– or <em>y</em>-coordinates.

In this project, you are asked to scan an input set of points in the plane in the order of their polar angles with respect to a reference point. This reference point, called the <em>median coordinate point </em>(MCP), has its <em>x</em>-coordinate equal to the median of the <em>x</em>-coordinates of the input points and its <em>y</em>-coordinate equal to the median of their <em>y</em>-coordinates. A polar angle with respect to this point lies in the range [0<em>,</em>2<em>π</em>).

Finding the median <em>x</em>– and <em>y</em>-coordinates is done by sorting the points separately by the corresponding coordinate.<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> Once the MCP has been constructed, a third round of sorting will be conducted by the polar angle with respect to this point.

You need to scan the input points four times, each time using one of the four sorting algorithms presented in class: selection sort, insertion sort, merge sort, and quicksort. Note that the <strong>same sorting algorithm </strong>must be used in all three rounds of sorting within one scan.

We make the following two assumptions:

<ul>

 <li>All input points have <strong>integer </strong>coordinates ranging between −50 and 50, inclusive.</li>

 <li>The input points may have <strong>duplicates</strong>.</li>

</ul>

Integer coordinates are assumed to avoid issues with floating-point arithmetic. The rectangular range [−50<em>,</em>50] × [−50<em>,</em>50] is big enough to contain 10,201 points with integer coordinates.

Since the input points will be either generated as pseudo-random points or read from an input file, duplicates may appear. Also, it is unnecessary to test your code on more than 10,201 input points (otherwise duplicates will certainly arise).

1.1         Point Class and Comparison Methods

The Point class implements the Comparable interface. Its compareTo() method compares the <em>x</em>or <em>y</em>– coordinates of two points. In case two points tie on their values of the selected coordinate, the values of the other coordinate are compared to break the tie.

Point comparison can also be done using an object of the PolarAngleComparator class, which you are required to implement. The polar angle is with respect to a point stored in the instance variable referencePoint. The compare() method in this class must be implemented using cross and dot products, not any trigonometric or square root functions. You need to handle special situations where multiple points are equal to referencePoint, have the same polar angle with respect to it, etc. Please read the Javadoc for the methods compare() and comparePolarAngle() carefully.

1.2         Sorter Classes

Selection sort, insertion sort, merge sort, and quicksort are respectively implemented by the classes

SelectionSorter, InsertionSorter, MergeSorter, and QuickSorter, all of which extend the abstract class AbstractSorter. The abstract class has one constructor awaiting your implementation:

protected AbstractSorter(Point[] pts) throws IllegalArgumentException

The constructor takes an existing array pts[] of points and copies it over to the array points[].

It throws an IllegalArgumentException if pts == null or pts.length == 0.

Besides having an array points[] to store points, AbstractSorter also includes three instance variables.

<ul>

 <li>algorithm: type of sorting algorithm to be initialized by a subclass constructor.</li>

 <li>pointComparator: comparator used for point comparison. Set by calling setComparator(). It compares two points by their <em>x</em>-coordinates, <em>y</em>-coordinates, or polar angles with respect to referencePoint.</li>

 <li>referencePoint: reference point that polar angles are taken with respect to. Set by calling setReferencePoint().</li>

</ul>

The method sort() conducts sorting, for which the algorithm is determined by the dynamic type of the AbstractSorter. The method setComparator() must have been called beforehand to generate an appropriate comparator for sorting by the <em>x</em>-coordinate, <em>y</em>-coordinate, or polar angle. In the case that the polar angle is used, referencePoint must have a value that is not null.<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>

The class also provides two methods: getPoints() to get the contents of the array points[], and getMedian() to return the element with the median index in points[].

Each of the four subclasses SelectionSorter, InsertionSorter, MergeSorter, and QuickSorter has a constructor that needs to call the superclass constructor.

<strong>1.3 </strong>RotationalPointScanner <strong>Class</strong>

This class has two constructors. Both accept one type of sorting algorithm. The first constructor reads points from an array. The second one reads points from an input file of integers, where each pair of integers represents the <em>x </em>and <em>y</em>-coordinates of one point. A FileNotFoundException will be thrown if no file by the inputFileName exists, and an InputMismatchException will be thrown if the file consists of an odd number of integers.<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>

For example, suppose a file points.txt has the following content:

0 0 -3 -9 0 -10

8 4 3 3 -6

3 -2 1

10 5 -7 -10

5 -2

7 3 10 5

-7 -10 0 8

-1 -6

-10 0 5 5

There are 34 integers in the file. A call

RotationalPointScanner(points.txt, Algorithm.QuickSort);

will initialize the array points[] to store the following 17 points, which are plotted<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a> below, along with their MCP: (0<em>,</em>0), (−3<em>,</em>−9), (0<em>,</em>−10), (8<em>,</em>4), (3<em>,</em>3), (−6<em>,</em>3), (−2<em>,</em>1), (10<em>,</em>5), (−7<em>,</em>−10), (5<em>,</em>−2), (7<em>,</em>3), (10<em>,</em>5), (−7<em>,</em>−10), (0<em>,</em>8), (−1<em>,</em>−6), (−10<em>,</em>0), and (5<em>,</em>5).

8

6

4

2

0

−4

−6 −8

−10

<em>x</em>

Note that the points (-7, -10) and (10, 5) each appear twice in the input.

The 17 points have <em>x</em>-coordinates in the following sequence:

0<em>,</em>−3<em>,</em>0<em>,</em>8<em>,</em>3<em>,</em>−6<em>,</em>−2<em>,</em>10<em>,</em>−7<em>,</em>5<em>,</em>7<em>,</em>10<em>,</em>−7<em>,</em>0<em>,</em>−1<em>,</em>−10<em>,</em>5

which, after sorted in the non-decreasing order, becomes,

−10<em>,</em>−7<em>,</em>−7<em>,</em>−6<em>,</em>−3<em>,</em>−2<em>,</em>−1<em>,</em>0<em>,</em>0<em>,</em>0<em>,</em>3<em>,</em>5<em>,</em>5<em>,</em>7<em>,</em>8<em>,</em>10<em>,</em>10

Since the largest index in the private array points[] storing the points is 16, the median is 0 at the index 16<em>/</em>2 = 8. (Note that integer division in Java truncates the fractional part, if any, of the result.) Similarly, the above points have <em>y</em>-coordinates in the following sequence:

0<em>,</em>−9<em>,</em>−10<em>,</em>4<em>,</em>3<em>,</em>3<em>,</em>1<em>,</em>5<em>,</em>−10<em>,</em>−2<em>,</em>3<em>,</em>5<em>,</em>−10<em>,</em>8<em>,</em>−6<em>,</em>0<em>,</em>5

which is in the non-decreasing order below.

−10<em>,</em>−10<em>,</em>−10<em>,</em>−9<em>,</em>−6<em>,</em>−2<em>,</em>0<em>,</em>0<em>,</em>1<em>,</em>3<em>,</em>3<em>,</em>3<em>,</em>4<em>,</em>5<em>,</em>5<em>,</em>5<em>,</em>8

The median <em>y</em>-coordinate is 1 at the index 8. The median coordinate point (MCP) is therefore (0<em>,</em>1), colored blue in the plot. Notice that it does not coincide with any of the input points.

Construction of the MCP is carried out by the method scan() of the class by sorting the points by <em>x</em>– and <em>y</em>-coordinates, respectively. After these two sorting rounds, the method sets the value of medianCoordinatePoint to the MCP, and then calls the method setReferencePoint() of the AbstractSorter class to set the value of its instance variable referencePoint to the MCP. The method scan() then carries out a final round of sorting by polar angle with respect to the MCP.

Besides using an array points[] to store points and medianCoordinatePoint to store the MCP, the RotationalPointScanner class also includes several other instance variables.

<ul>

 <li>sortingAlgorithm: type of sorting algorithm. Initialized by a constructor.</li>

 <li>outputFileName: name of the file to store the sorting result in: txt, insert.txt, merge.txt, or quick.txt.</li>

 <li>scanTime: sorting time in nanoseconds. This sums up the times spent on three rounds of sorting. Within sort(), use the nanoTime() method.</li>

</ul>

In the previous example, after calling scan(), the array points[] will store the points ordered by the polar angle with respect to the MCP: (7<em>,</em>3), (8<em>,</em>4), (10<em>,</em>5), (10<em>,</em>5), (3<em>,</em>3), (5<em>,</em>5), (0<em>,</em>8), (−6<em>,</em>3), (−2<em>,</em>1), (−10<em>,</em>0), (−7<em>,</em>−10), (−7<em>,</em>−10), (−3<em>,</em>−9), (−1<em>,</em>−6), (0<em>,</em>0), (0<em>,</em>−10), (5<em>,</em>−2).

Among them, (0<em>,</em>0) and (0<em>,</em>−10) have the same polar angle. They are thus ordered by distance to this point.

2           Compare Sorting Algorithms

The class CompareSorters uses the class RotationalPointScanner to scan points randomly generated or read from files four times, each time using a different sorting algorithm. Multiple input rounds are allowed. In each round, the main() method compares the execution times of the four scans of the same input sequence. The round proceeds as follows:

<ol>

 <li>Create an array of randomly generated integers, if needed.</li>

 <li>Construct four RotationalPointScanner objects over the point array, each with a different algorithm (i.e., a different value for the parameter algo of the constructor).</li>

 <li>Have every created RotationalPointScanner object call scan().</li>

 <li>At the end of the round, output the statistics by having every RotationalPointScanner object call the stats()</li>

</ol>

Below is a sample execution sequence with running times.

Performances of Four Sorting Algorithms in Point Scanning keys: 1 (random integers) 2 (file input) 3 (exit)

Trial 1: 1

Enter number of random points: 1000 algorithm size time (ns)

———————————-

SelectionSort 1000 49631547

InsertionSort 1000 22604220

MergeSort 1000 2057874

QuickSort 1000 1537183

———————————-

Trial 2: 2

Points from a file File name: points.txt algorithm size time (ns)

———————————-

SelectionSort 1000 3887008

InsertionSort 1000 9841766

MergeSort 1000 1972146

QuickSort 1000 888098

———————————…

Your code needs to print out the same text messages for user interactions. Entries in every column of the output table should be aligned.

3           Random Point Generation

To test your code, you may generate random points within the range [−50<em>,</em>50] × [−50<em>,</em>50]. Each random point has its <em>x</em>– and <em>y</em>-coordinates generated separately as pseudo-random numbers within the range [−50<em>,</em>50]. You already had experience with random number generation from Project 1. Import the Java package java.util.Random. Next, declare and initiate a Random object with

Random generator = new Random();

Then, the expression generator.nextInt(101) 50 will generate a pseudo-random number between -50 and 50 every time it is executed.

4           Display of a Point Scan

The sorted points will be displayed using Java graphics package Swing. The display will help you visually check that the points are correctly sorted. The fully implemented class Plot is for this purpose. A few things about the implementation to note below:

<ul>

 <li>The JFrame class is a top level container that embodies the concept of a “window.” It allows you to create an actual window with customized attributes like size, font, color, etc. It can display one or more JPanel objects at the same time.</li>

 <li>JPanel is for painting and drawing, and must be added to the JFrame to create the display. A JPanel represents some area in a JFrame in which controls such as buttons and text fields and visuals such as figures, pictures, and text can appear.</li>

 <li>The Graphics class may be thought of like a pen that does the actual drawing. The class is abstract and often used to specify a parameter of some method (in particular, paint()). This parameter is then downcast to a subclass such as Graphics2D for calling the latter’s utility methods.</li>

 <li>The paint() method is called automatically when a window is created. It must be overridden to display your drawings.</li>

</ul>

The results of the four scans are displayed in separate windows. For display, a separate thread is created inside the method myFrame() in the class Plot. Please do not modify the Plot class for better display unless you understand what is going on there.

The class Segments has been implemented for creating specific line segments to connect the input points so you can see the correctness of the sorting result.

4.1         Drawing Data Preparation

The output of each scan can be displayed by calling the partially implemented method draw() in the RotationalPointScanner class, <strong>only after </strong>scan() is called, via the statement

Plot.myFrame(points, segments, sort);

where the first two parameters have types Point[] and Segment[], respectively, and the third parameter is a string name for the sorting algorithm used. By this time, the array points[] stores the points in the order of scan. You will need to create an array segments[] to store some line segments which, when drawn, can visually reveal the order among the points. Create line segments to connect:

<ul>

 <li>every pair of consecutive points in points[] that are distinct from each other, • the first (index 0) and last points in points[] if they are distinct, and</li>

 <li>medianCoordinatePoint to every point in points[].</li>

</ul>

The order among the elements in segments[] may be arbitrary. Consider the same 17 input points (with duplicates) that were plotted above. After a scan, segments[] would consist of the following 30 segments, the last 15 of which share an endpoint at medianCoordinatePoint == (0, 1):

<table width="0">

 <tbody>

  <tr>

   <td width="121">((7<em>,</em>3)<em>,</em>(8<em>,</em>4))</td>

   <td width="102">((8<em>,</em>4)<em>,</em>(10<em>,</em>5))</td>

   <td width="108">((10<em>,</em>5)<em>,</em>(3<em>,</em>3))</td>

   <td width="124">((3<em>,</em>3)<em>,</em>(5<em>,</em>5))</td>

   <td width="114">(5<em>,</em>5)<em>,</em>(0<em>,</em>8))</td>

  </tr>

  <tr>

   <td width="121">((0<em>,</em>8)<em>,</em>(−6<em>,</em>3))</td>

   <td width="102">((−6<em>,</em>3)<em>,</em>(−2<em>,</em>1))</td>

   <td width="108">((−2<em>,</em>1)<em>,</em>(−10<em>,</em>0))</td>

   <td width="124">((−10<em>,</em>0)<em>,</em>(−7<em>,</em>−10))</td>

   <td width="114">((−7<em>,</em>−10)<em>,</em>(−3<em>,</em>−9))</td>

  </tr>

  <tr>

   <td width="121">((−3<em>,</em>−9)<em>,</em>(−1<em>,</em>−6))</td>

   <td width="102">((−1<em>,</em>−6)<em>,</em>(0<em>,</em>0))</td>

   <td width="108">((0<em>,</em>0)<em>,</em>(0<em>,</em>−10))</td>

   <td width="124">((0<em>,</em>−10)<em>,</em>(5<em>,</em>−2))</td>

   <td width="114">((5<em>,</em>−2)<em>,</em>(7<em>,</em>3))</td>

  </tr>

  <tr>

   <td width="121">((0<em>,</em>1)<em>,</em>(7<em>,</em>3))</td>

   <td width="102">((0<em>,</em>1)<em>,</em>(8<em>,</em>4))</td>

   <td width="108">((0<em>,</em>1)<em>,</em>(10<em>,</em>5))</td>

   <td width="124">((0<em>,</em>1)<em>,</em>(3<em>,</em>3))</td>

   <td width="114">((0<em>,</em>1)<em>,</em>(5<em>,</em>5))</td>

  </tr>

  <tr>

   <td width="121">((0<em>,</em>1)<em>,</em>(0<em>,</em>8))</td>

   <td width="102">((0<em>,</em>1)<em>,</em>(−6<em>,</em>3))</td>

   <td width="108">((0<em>,</em>1)<em>,</em>(−2<em>,</em>1))</td>

   <td width="124">((0<em>,</em>1)<em>,</em>(−10<em>,</em>0))</td>

   <td width="114">((0<em>,</em>1)<em>,</em>(−7<em>,</em>−10))</td>

  </tr>

  <tr>

   <td width="121">((0<em>,</em>1)<em>,</em>(−3<em>,</em>−9))</td>

   <td width="102">((0<em>,</em>1)<em>,</em>(−1<em>,</em>−6))</td>

   <td width="108">((0<em>,</em>1)<em>,</em>(0<em>,</em>0))</td>

   <td width="124">((0<em>,</em>1)<em>,</em>(0<em>,</em>−10))</td>

   <td width="114">((0<em>,</em>1)<em>,</em>(5<em>,</em>−2))</td>

  </tr>

 </tbody>

</table>

<h2>4.2         Displaying the Result From a Scan</h2>

8

6

4

2

0

−4

−6

−8

−10

<em>x</em>

For the 30 segments listed in Section 4.1, the corresponding display window will show a triangulation like the one above. (You don’t need to use different colors to emulate this plot.)

The outer boundary of the triangulation is a simple polygon (drawn in blue) with the 15 distinct input points as vertices. A counterclockwise traversal of the polygon vertices starting at (7<em>,</em>3) will never decrease the polar angle with respect to the median coordinate point (0<em>,</em>1). Fifteen orange line segments connect (0<em>,</em>1) to the vertices. If your scanning result is correct, no two line segments should intersect at a point in their interiors.

.








