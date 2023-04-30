Download Link: https://assignmentchef.com/product/solved-cs251-data-structures-project-06-part-1-of-2-hashmap
<br>
<span style="font-size: 2em;">Overview</span>

The goal is an extensible <strong>hashmap&lt;KeyT, ValueT&gt;</strong> class that implements hashing, handling collisions using probing.  The hashmap represents part 1 of the project.  In part 2 of the project, you’ll use your hashmap to analyze data from the <strong>Divvy</strong> company.  This handout focuses on part 1 only.




On the course <a href="https://www.dropbox.com/sh/1hnv1z2k2dv43p7/AADJ1aYXTdwfaUyQLwRZy1kVa?dl=0">dropbox</a> (and Codio), a solution is provided to the lab from week #11.  Recall that this was the lottery data program.  The provided solution is written using a <strong>hashmap</strong> class, much like the one you need to write.  In fact, your solution here in part 1 should start with the provided implementation.  The hashmap class allocates the hash table, and provides the <strong>insert()</strong> and <strong>search()</strong> functions.  Interestingly, the <strong>Hash()</strong> function remains separate and *outside* the class, allowing the hashmap to be used in other scenarios.  Here’s code for main() rewritten to use hashmap:




hashmap&lt;string, LotteryData&gt; <strong>hmap</strong>(37200);

.

. // insert data into hmap from csv file: .

cin &gt;&gt; thedate; while (thedate != “#”)

{

LotteryData  ld;

if (<strong>hmap.search(thedate, ld, Hash)</strong>)  // notice Hash() function passed as param:

{

cout &lt;&lt; “Winning numbers: ” &lt;&lt; ld.Numbers &lt;&lt; endl;       cout &lt;&lt; “Mega ball: ”       &lt;&lt; ld.MegaBall &lt;&lt; endl;       cout &lt;&lt; “Multiplier: ”      &lt;&lt; ld.Multiplier &lt;&lt; endl;

}    else // not found:

{

cout &lt;&lt; “Sorry, no data for this date” &lt;&lt; endl;

}

cin &gt;&gt; thedate;

}//while

Review the provided code for the complete details, but the keys aspects to notice are the following:




<ol>

 <li>The hashmap stores the (key, value) pairs</li>

 <li>The hashmap does hashing inside the insert() and search() functions</li>

 <li>The Hash() function is passed as a parameter to the insert() and search() functions</li>

</ol>




The last one is the most interesting, since this means you can have different hashmaps storing different data using different hash functions.  That’s what we meant by an “extensible” hashmap class.

<h2>Getting started</h2>

You’ll want to review the provided solution carefully.  As we start to program using multiple .h and .cpp files (which is how most C/C++ programs are written), you’ll want to appreciate — and correctly utilize — this

approach.  Build the program and convince yourself it works.  You can even submit the program to Gradescope (“<strong>Lab Week 11 – testing only</strong>”) and it should score 100.




The provided files are available on the course dropbox under Projects, <a href="https://www.dropbox.com/sh/1hnv1z2k2dv43p7/AADJ1aYXTdwfaUyQLwRZy1kVa?dl=0">project06</a><a href="https://www.dropbox.com/sh/1hnv1z2k2dv43p7/AADJ1aYXTdwfaUyQLwRZy1kVa?dl=0">–</a><a href="https://www.dropbox.com/sh/1hnv1z2k2dv43p7/AADJ1aYXTdwfaUyQLwRZy1kVa?dl=0">files</a><a href="https://www.dropbox.com/sh/1hnv1z2k2dv43p7/AADJ1aYXTdwfaUyQLwRZy1kVa?dl=0">.</a>  A Codio project is also setup:  <strong>cs251-project06-hashing</strong>.  The provided solution consists of two .h files and two .cpp files:







As you work with multiple files, keep in mind that we are building *one* program.  So any function in any C++ file can be called from another other C++ file.  <u>Example</u>:  the “hash.cpp” file contains 2 functions:  <strong>isNumeric()</strong> and <strong>Hash()</strong>.  Folks wanted to take advantage of the isNumeric() function in “main.cpp” — which is great.  But the mistake is that they copypasted the code from hash.cpp to main.cpp.  Whenever you copy-paste, an alarm should go off in your head — “this is the wrong approach”.  In this case, since the function already exists in “hash.cpp”, what you want to do instead is just call it from “main.cpp”.  To do that, you make it available by placing the function header in “hash.h”:




int isNumeric(string s);




Now you just call the function from main.cpp.  [ <em>BTW, this is why .h files are called header files, they typically contain function headers.</em> ]




You might be wondering, “<strong>Why does hashmap.h contain both function headers *and* C++ code?  Why isn’t the C++ code for hashmap in a separate .cpp file?</strong>”  Great question.  Since hashmap is templated, the C++ code must reside in the .h file; it’s a current limitation of C++.  Normally, classes follow the same rules as other files:  header information in the .h file, and C++ implementation code in the .cpp file.  But templated classes are an exception.

<h2>Assignment — handling collisions</h2>

Your assignment here in part 1 is to re-write the <strong>hashmap</strong> class to support collisions using probing.  In other words, from the outside — when called from main() — the hashmap class should behave exactly as before.  The <strong>insert()</strong> function inserts, and the <strong>search()</strong> function searches.  The fact that collisions are occurring should make no difference to the main() program.




However, the provided hashmap class is not written to handle collisions; it assumes a perfect hash function.  If the Hash() function causes collisions, the hashmap will fail to function properly (overwriting values, returning wrong values, etc.).  This implies you need to rewrite the <strong>insert()</strong> and <strong>search()</strong> functions to handle collisions using probing.




Recall that lecture 29 (Friday 4/3) discussed how to handle collisions using chaining and probing.  You should watch or re-watch this recording before starting to program.  The recording can be found on BB under “lecture recordings”.




This is not meant to be a difficult exercise; perhaps 50 lines of code.  In fact, the only changes required are to the following aspects of “hashmap.h”:




<ol>

 <li><em>Update the header comment since it’s misleading. The class does not require a perfect hash function, and the class now handles collisions.  Add your name while you’re there. </em></li>

 <li><em>Update the insert() function to properly handle collisions using probing. Note that if the (key, value) is already in the hash table, the existing value is overwritten by the new value.  You may assume the hash table will never fill, and you will eventually find an empty location to insert. </em></li>

 <li><em>Update the search() function to properly handle collisions. You may assume the hash table will never fill, and you will eventually reach an empty location. </em></li>

 <li><em>Add a copy constructor to properly make a deep copy. </em></li>

 <li><em>Overload operator= to properly free memory and make a deep copy. </em></li>

</ol>

How to test?  Interestingly, all you need to do is start generating collisions, and see how the program behaves.  If the program behaves exactly as before then the hashmap is handling collisions properly.  If not, then something is wrong.  The easiest way to generate collisions is to reduce the size of hash table, in this case to a value &lt; 37,200.  The smaller the size, the more collisions (although be sure there’s enough room to insert all the data).  There are roughly 2,000 lines of data in the .csv file, so start by trying maybe a size of 30,000.  A simple change to the top of main() is all you need:




int main()

{

cout &lt;&lt; “** Mega millions lottery data **” &lt;&lt; endl;  cout &lt;&lt; endl;




//

// Use a smaller array size now to generate collisions:

//

const int N = <strong>30000</strong>;

hashmap&lt;string, LotteryData&gt; hmap(N);




Run and test locally, entering a few dates.  The program should work as before.  Then submit to gradescope (“<strong>Lab Week 11 – testing only</strong>”), and the score should remain a 100.  Repeat the exercise by reducing the array size further.  As long as N &gt; 2000, the program should work as before — the only difference is that it’s running slower as the # of collisions increase.




As for testing the copy constructor and operator=, you’ll need to write some code to test your implementations.  Here are the proper function declarations to add to “hashmap.h”:




//

// copy constructor:

//

hashmap(const hashmap&amp; other)

{

//

// TODO:

//

}




//

// operator=

//

hashmap&amp; operator=(const hashmap&amp; other)

{

//

// TODO:

//

return *this;

}










<h2>Interesting side notes</h2>

There are two aspects of the program that you might find interesting.  First, how is the <strong>Hash()</strong> function passed to the Insert() and Search() functions?  In C, you would have to use function pointers, which are notoriously unsafe and error-prone.  C++ once again takes a more modern approach and hides the pointers from you.  Instead, we are working with objects of type <strong>std::function</strong>.




Open “hashmap.h” and look at the header declaration of <strong>insert()</strong>.  You’ll see the Hash parameter declared as following:




bool <strong>insert</strong>(KeyT key, ValueT value, <strong>function&lt;int(KeyT,int)&gt; Hash</strong>)




This declares <strong>Hash</strong> as a function of type <em>int(KeyT, int)</em>.  This means the Hash() function must return an int, and take 2 paramters:  a key and an int.  Thinking in terms of the lottery program, look at the type of the actual Hash() function in “hash.cpp”:




int Hash(string theDate, int N)




This matches the required type declaration, since the keys are dates — i.e. strings.




Second, look at the Hash() function in “hash.cpp”.  Note that nearly all of the if statements are gone, and replaced by a <strong>regular expression</strong>.   Regular expressions are a common technique for validation, and as you can see much simpler and cleaner.  It requires learning a new language — regular expressions — but it’s a very powerful technique.  We’ll discuss regular expressions in class on Monday April 6<sup>th</sup>; you’ll also learn much more about REs in CS 301.

<h2>Requirements</h2>

<ol>

 <li>The hashmap class must use hashing, and probing. You cannot use chaining of any type.</li>

 <li>You cannot use any of the built-in data structures; you must continue to use the given dynamic array of structures. You may assume the hash table will never fill (i.e. that N was large enough when the hashmap was first constructed).</li>

 <li>Add a copy constructor, and properly overload operator=.</li>

 <li>You are required to free all memory allocated by your class. Valgrind will be used to confirm that memory has been properly freed.</li>

 <li>No global variables.</li>

 <li>If any of the requirements are broken, the grade is a 0.</li>

</ol>

<h2>Grading, electronic submission, and Gradescope</h2>

<strong><u>Submission</u></strong>:  “hashmap.h”.




Your score on this project is based on two factors:  (1) correctness of hashmap.h” as determined by Gradescope, and (2) manual review of “hashmap.h” for commenting, style, and approach (e.g. adherence to requirements).  Since this is part 1, it is worth 50 points for correctness, and 10 points for manual review of commenting and style.  In this class we expect all submissions to compile, run, and pass at least some of the test cases; do not expect partial credit with regards to correctness.  <u>Example</u>:  if you submit to Gradescope and your score is a reported as a 0, then that’s your correctness score.  The only way to raise your correctness score is to re-submit.




In this project, your “hashmap.h” will be allowed <strong>12 free submissions</strong>.  After 12 submissions, each additional submission will cost 1 point.  <u>Example</u>: suppose you score 50 after 15 submissions, and you activate this submission for grading.  Your autograder score will be 47:  50 – 3 extra submissions.  Note that you cannot use another student’s account to test your work; this is considered academic misconduct because you have given your code to another student for submission on their account.




By default, we grade your <strong><u>last</u></strong> submission.  Gradescope keeps a complete submission history, so you can <strong><u>activate</u></strong> an earlier submission if you want us to grade a different one; this must be done before the due date.  We assume *every* submission on your Gradescope account is your own work; do not submit someone else’s work for any reason, otherwise it will be considered academic misconduct.

<h2>Policy</h2>

Late work *is* accepted.  You may submit as late as 24 hours after the deadline for a penalty of 10%.  After 24 hours, no submissions will be accepted.




All work submitted for grading *must* be done individually.  While we encourage you to talk to your peers and learn from them (e.g. your “iClicker teammates”), this interaction must be superficial with regards to all work submitted for grading.  This means you *cannot* work in teams, you cannot work side-by-side, you cannot submit someone else’s work (partial or complete) as your own.  The University’s policy is available here:




<a href="https://dos.uic.edu/conductforstudents.shtml">https://dos.uic.edu/conductforstudents.shtml</a> .




In particular, note that you are guilty of academic dishonesty if you <u>extend or receive any kind of</u> <u>unauthorized assistance</u>.  Absolutely no transfer of program code between students is permitted (paper or electronic), and you may not solicit code from family, friends, or online forums.  Other examples of academic dishonesty include emailing your program to another student, copying-pasting code from the internet, working in a group on a homework assignment, and allowing a tutor, TA, or another individual to write an answer for you.  It is also considered academic dishonesty if you click someone else’s iClicker with the intent of answering for that student, whether for a quiz, exam, or class participation.  Academic dishonesty is unacceptable, and penalties range from a letter grade drop to expulsion from the university; cases are handled via the official student conduct process described at <a href="https://dos.uic.edu/conductforstudents.shtml">https://dos.uic.edu/conductforstudents.shtml</a> .


