<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name: SANIYA G</h3>
<h3>Register Number:212223240147 </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor
        i) if successor is the goal, stop search
        
        ii) else, compute both g and h for successor
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
          successor.f = successor.g + successor.h

        iii) if a node with the same position as 
            successor is in the OPEN list which has a 
           lower f than successor, skip this successor

        iV) if a node with the same position as 
            successor  is in the CLOSED list which has
            a lower f than successor, skip this successor
            otherwise, add  the node to the open list
     end (for loop)
  
    e) push q on the closed list
    end (while loop)

``````
## PROGRAM :
`````python
#!/usr/bin/env python
# coding: utf-8

from collections import defaultdict

# Global variables
H_dist = {}
Graph_nodes = {}   # ✅ define globally so it won’t give “not defined” errors

def aStarAlgo(start_node, stop_node):
    open_set = set(start_node)
    closed_set = set()
    g = {}               # store distance from starting node
    parents = {}         # parents contains an adjacency map of all nodes
    
    # distance of starting node from itself is zero
    g[start_node] = 0
    # start_node is root node i.e it has no parent
    parents[start_node] = start_node
    
    while len(open_set) > 0:
        n = None
        
        # node with lowest f() = g() + h() is found
        for v in open_set:
            if n is None or g[v] + heuristic(v) < g[n] + heuristic(n):
                n = v
        
        if n is None:
            print("Path does not exist!")   # ✅ fixed
            return None

        if n == stop_node or Graph_nodes[n] is None:
            pass
        else:
            for (m, weight) in get_neighbors(n):
                if m not in open_set and m not in closed_set:
                    open_set.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight
                else:
                    if g[m] > g[n] + weight:
                        g[m] = g[n] + weight
                        parents[m] = n
                        if m in closed_set:
                            closed_set.remove(m)
                            open_set.add(m)
        
        # if the current node is the stop_node
        if n == stop_node:
            path = []
            while parents[n] != n:
                path.append(n)
                n = parents[n]
            path.append(start_node)
            path.reverse()
            print('Path found: {}'.format(path))
            return path
        
        open_set.remove(n)
        closed_set.add(n)
    
    print("Path does not exist!")
    return None


# define function to return neighbor and its distance
def get_neighbors(v):
    """
    Retrieves neighbors and edge weights from Graph_nodes.
    """
    if v in Graph_nodes:
        return Graph_nodes[v]   # ✅ fixed
    else:
        return None


def heuristic(n):
    return H_dist[n]


# ---------------- MAIN ----------------
if __name__ == "__main__":
    graph = defaultdict(list)
    n, e = map(int, input("Enter number of nodes and edges: ").split())

    for i in range(e):
        u, v, cost = map(str, input("Enter edge (u v cost): ").split())
        t = (v, float(cost))
        graph[u].append(t)
        t1 = (u, float(cost))
        graph[v].append(t1)

    for i in range(n):
        node, h = map(str, input("Enter node and heuristic: ").split())
        H_dist[node] = float(h)

    print("Heuristic values:", H_dist)

    Graph_nodes = graph
    print("Graph:", dict(graph))

    start = input("Enter start node: ")
    goal = input("Enter goal node: ")
    aStarAlgo(start, goal)


`````
<hr>
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

<hr>
<h2>Sample Input</h2>
<hr>
10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'F', 'G', 'I', 'J']

### Output:


<img width="1474" height="809" alt="image" src="https://github.com/user-attachments/assets/2ab8f999-509d-4bb6-98d0-9cef702b7216" />
<img width="1919" height="301" alt="image" src="https://github.com/user-attachments/assets/6bf08f6b-7ada-48fd-afac-39478ef41b2a" />







<hr>
<h2>Sample Graph II</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)




<hr>
<h2>Sample Input</h2>
<hr>
6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'E', 'D', 'G']


### Output:


<img width="1919" height="779" alt="image" src="https://github.com/user-attachments/assets/cc51588a-91fc-4488-9121-2f3097687ce1" />




## Result : 

Thus the A* algorithm has been verified successfully.
