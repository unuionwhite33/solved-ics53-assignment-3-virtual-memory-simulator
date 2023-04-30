Download Link: https://assignmentchef.com/product/solved-ics53-assignment-3-virtual-memory-simulator
<br>
You will implement a VM Simulator which will simulate the operation of a virtual memory system. To launch the system, your simulator will accept one optional command-line argument for the selection of  the page replacement algorithm. The system will then accept commands to read/write from/to a virtual address space. Your system will need to correctly move pages between disk and main memory in order to satisfy access requests. Your system will need to properly maintain the page table as part of this process.

<h1>Parameters of the Virtual Memory System</h1>

Each address in memory stores a single integer. All memory locations are initialized to the value of -1. All virtual pages are initially on disk, so the valid bits of all page table entries are equal to 0.

Your virtual memory system will have 32 addresses (0 – 31) and main memory has 16 addresses (0 – 15). Each page contains 4 addresses, so your virtual memory system will have 8 pages and your main memory will have 4 pages. Pages are numbered sequentially starting at the lowest memory address. So page 0 contains addresses 0-3, page 1 contains addresses 4-7, and so on. And the disk page number of each virtual page is the same as the virtual page number.

<h1>User Interface of VM Simulator</h1>

The user will type in a sequence of commands and your program will perform the operation specified by each command. Your program will execute in a loop which starts with your program printing a  “&gt; “ prompt at the beginning of the line in order to let the user know that your program is ready to receive a new command. The user will type in a command, followed by any necessary arguments, and then hit the ‘enter’ key to indicate that the command is complete. Your program will then execute the command, print data to the screen if necessary, print a “&gt; “ prompt on a new line to receive a new command. (Note: “&gt; “ is one &gt; and one space.)

<h2>Commands</h2>

Your program must (and will only) process the following user commands.

<ol>

 <li><strong>read &lt;virtual_addr&gt;</strong>​ :​ This command prints the contents of a memory address. The command takes one argument which is the virtual address of the memory location to be read. When a page fault occurs, “A Page Fault Has Occurred
” is printed to the screen before the contents of a memory address.</li>

 <li><strong>write &lt;virtual_addr&gt; &lt;num&gt;</strong>​ :​ The command writes data to a memory location. The command takes two arguments, the virtual address to write the data to, and an integer which will be written to the address. When a page fault occurs, “A Page Fault Has Occurred
” is printed to the screen.</li>

 <li><strong>showmain &lt;ppn&gt;</strong>​ :​ This command prints the contents of a physical page in main memory. The command takes one argument which is the number of the physical page to be printed. Since each page contains four addresses, four contents of addresses should be printed and together with their associated addresses. You can see the format from the example below which shows the contents of physical page 1 that includes the value 100 at address 4, the value 101 at address 5, the value 102 at address 6, and the value 103 at address 7.</li>

</ol>

4:100

5:101

6:102

7:103

<ol start="4">

 <li><strong>showdisk &lt;dpn&gt;</strong>​ :​ This command prints the contents of a page on disk. The command takes one argument which is the number of the disk page to be printed. Since each page contains four addresses, four contents of addresses should be printed and together with their associated addresses. The format for printing the contents of the disk page is the same as the <strong>showmain </strong>​        ​</li>

 <li><strong>showptable</strong>​ :​ This command prints the contents of the page table. Your virtual memory system contains 8 virtual pages, so this command will print 8 page table entries. Each page table entry contains three fields of information about a page.

  <ul>

   <li><em>Valid bit</em>​: 1 indicates that the corresponding page is in main memory and 0 indicates that the page is on disk.</li>

  </ul></li>

</ol>




<ul>

 <li><em>Dirty bit</em>​: 1 indicates that the corresponding page has been written to while in main memory. 0 indicates that the page has not been written to since it has been in main memory. The dirty bit has no meaning if the page is not in main memory, i.e. if the Valid bit is 0.</li>

</ul>




<ul>

 <li><em>Page Number (PN)</em>​: This is an integer indicating the number of the page (in main memory or disk) where the virtual page can be found in memory. The PN refers to a main memory physical page when the Valid bit is equal to 1, and the PN refers to a disk page when the valid bit is 0.</li>

</ul>




The <strong>showptable </strong>​            command will print the contents of each page table entry on a separate line in the​         following format, where each line is divided by colons with four entries, virtual page number, <em>Valid</em>​        <em> bit</em>​, <em>Dirty bit</em>​     ​, and <em>PN</em>​ ​. Below is an example output of three-page page table from the <strong>showptable</strong>​   command. For instance, the first line represents that “At virtual page number 0, <em>Valid bit</em>​             ​ is 1 and <em>Dirty bit</em>​ is 0, and <em>Page Number</em>​    ​ is 0.”

0:1:0:0

1:1:1:3

2:0:0:2

…

Although the above example only shows 3 pages for brevity, your <strong>showptable </strong>​ command should​            print all 8 pages from the page table.

<ol start="6">

 <li><strong>quit</strong>​ :​ This command quits the program. (Note: No need to print anything.)</li>

</ol>




<h1>Page Replacement Algorithm (Handling page faults)</h1>

When a page fault occurs, a disk page must be copied into main memory. If there is one or more page in main memory which is available (there is not virtual page mapped to it) then the disk page should be copied into the available main memory page with the lowest page number. If all pages in main memory are in use, then a victim page must be chosen for eviction from main memory.  You will implement two page replacement algorithms, FIFO and LRU, for the virtual memory system. And the page replacement will be selected at startup.

<ul>

 <li><strong>FIFO</strong>:​ The simplest algorithm is First-In-First-Out replacement. When a page fault occurs, the page that has been the longest in the main memory will be replaced.</li>

 <li><strong>LRU</strong>:​ The near-optimal algorithm is Least Recently Used replacement. When a page fault occurs, the page that is the least recently used/accessed in the main memory will be replaced.</li>

</ul>

Remember that the victim page must be copied back to disk before it is evicted if its own dirty bit is 1. When a victim page is copied back to disk, always copy it back to the disk page whose number is the same as the number of the virtual page.

You simulator should accept EITHER “<strong>FIFO</strong>​   ” or “​     <strong>LRU</strong>​     ” as the command-line argument OR no​             command-line argument, which by default adopts the FIFO algorithm. The following examples show how your simulator should launch the system. We assume that the linux prompt is the symbol “$ “ and the executable is named “a.out”.

<table width="624">

 <tbody>

  <tr>

   <td width="208">$ ./a.out</td>

   <td width="416">The system, by default, adopts the FIFO replacement algorithm.</td>

  </tr>

  <tr>

   <td width="208">$ ./a.out FIFO</td>

   <td width="416">The system adopts the FIFO replacement algorithm.</td>

  </tr>

  <tr>

   <td width="208">$ ./a.out LRU</td>

   <td width="416">The system adopts the LRU replacement algorithm</td>

  </tr>

 </tbody>

</table>




<h1>Example Usage</h1>

Following the above assumption, this is an example, launching the system with the FIFO replacement algorithm. The text typed by the user is in bold.

$ <strong>./a.out FIFO</strong>​

&gt; <strong>showptable</strong>​

0:0:0:0

1:0:0:1

2:0:0:2

3:0:0:3

4:0:0:4

5:0:0:5

6:0:0:6

7:0:0:7

&gt; <strong>read 9</strong>​

A Page Fault Has Occurred

-1

&gt; <strong>write 9 201</strong>​

&gt; <strong>read 9</strong>​

201

&gt; <strong>showmain 0</strong>​

8:-1

9:201

10:-1

11:-1

&gt; <strong>showptable</strong>​

0:0:0:0

1:0:0:1

2:1:1:0

3:0:0:3

4:0:0:4

5:0:0:5

6:0:0:6

7:0:0:7

&gt; <strong>quit</strong>​

$ <em>←you are back to the linux prompt</em>