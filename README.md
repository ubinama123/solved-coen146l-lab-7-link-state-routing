Download Link: https://assignmentchef.com/product/solved-coen146l-lab-7-link-state-routing
<br>
<h5>1.      To develop link state (LS) routing algorithm</h5>




<h5><strong>Network topology and data</strong></h5>

A network topology is defined by a set of nodes N and a set of edges E, formally defined as:

<ul>

 <li>N = set of nodes (routers)</li>

 <li>E = set of edges (links between connecting nodes)</li>

</ul>




It is assumed that the following information will be available for a network.

<ul>

 <li>Router ID, which is its index into the tables below, is given at the command line.</li>

 <li>Number of nodes, N, in the topology will be given by the command line.</li>

 <li>Table with costs C, NxN, will be obtained from file1 (name given at the command line).</li>

 <li>Table with machines, names, IP addresses, and port numbers, Nx3, will be obtained from file2 (name given at the command line).</li>

</ul>




Your main data will be:

<ul>

 <li>Neighbor cost table – contains the cost from every node to every node, initially obtained from file1.</li>

 <li>Least cost array – obtained with the link state algorithm.</li>

</ul>




<strong>LS routing protocol</strong>

Each router iteratively runs LS to find its forwarding table to every other node in the network (i.e. the shortest path tree) using Dijkstra algorithm. The pseudo code as described in the class textbook, is given as follows:




Data:

define N, E, C, and “empty” Nprime arrays

set source node to u

Define D(v) cost of path from source u to dest v

Define p(v) predecessor node along path from u to v




Initialization:

Nprime   ← ufor each node v in N:

If v adjacent to u

D(v) ← C(u,v     )

p(v) ← u

else

D(v) ← INFINITY

D(u) ← 0                        // Distance from source to source

Loop:while all nodes not in Nprime:

<em>node</em> ← <em>node</em> in N with min D(<em>node</em>)    // <em>node</em> with the least distance neighbor will be selected firstremove <em>node</em> from N and add to Nprimeupdate D(v) for all v adjacent to <em>node</em> and not in Nprime

D(v) ←  min (D(v), D(<em>node</em>)+C(<em>node</em>, v)(

p(v) ←  v (if D(v) is minimum, or <em>node </em>if D(<em>node</em>)+c(<em>node</em>, v) is minimum

Repeat




<strong>LS Code at each router</strong>

You may build your code with 3 threads, each with the following task:

<ul>

 <li>Thread 1 loops forever. It receives messages from other nodes and updates the neighbor cost table. When receiving a new cost c from x to neighbor y, it should update the cost in both costs: x to y and y to x.</li>

 <li>Thread 2 reads a new change from the keyboard every 10 seconds, updates the neighbor cost table, and sends messages to the other nodes using UDP. It finishes 30 seconds after executing 2 changes. You may execute this part in the main thread.</li>

 <li>Thread 3 loops forever. It sleeps for a random number of seconds (10-20), run the algorithm to update the least costs. After the algorithm executes it outputs the current least costs.</li>

</ul>




Make sure to use a mutex lock to synchronize the access to the neighbor cost table.




The messages between the routers will have 3 integers:

&lt;Routers’ ID&gt;&lt;neighbor ID&gt;&lt;new cost&gt;




The input for Thread 2, will have two integers:

&lt;neighbor&gt;&lt;space&gt;&lt;new cost&gt;&lt;new line&gt;




The table with costs will look like this if N = 3:

&lt;0,0&gt;               &lt;0,1&gt;               &lt;0,2&gt;

&lt;1,0&gt;               &lt;1,1&gt;               &lt;1,2&gt;

&lt;2,0&gt;               &lt;2,1&gt;               &lt;2,2&gt;

where &lt;i,j&gt; represents the cost between node i and node j. If &lt;i,j&gt; is equal to infinite (defined as 1,000), nodes i and j are not neighbors.




The table with machines will look like this, if N = 3.

&lt;machine0&gt; &lt;IP0&gt; &lt;port&gt;

&lt;machine1&gt; &lt;IP1&gt; &lt;port&gt;

&lt;machine2&gt; &lt;IP2&gt; &lt;port&gt;