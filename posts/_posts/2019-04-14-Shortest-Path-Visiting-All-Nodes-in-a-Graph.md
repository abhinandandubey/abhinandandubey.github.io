---
layout: post
title: Shortest Path Visting All Nodes in a Graph
tags: Interview, Programming
cover_url: https://upload.wikimedia.org/wikipedia/commons/thumb/d/d8/2011_Tour_de_France_map.png/1012px-2011_Tour_de_France_map.png
cover_meta: Tour De France Map, Wikimedia
color_scheme: tango
mathjax: true
---
<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>

## The Problem
We're given an undirected, but connected graph of $N$ nodes which are labeled $0, 1, 2, ..., N-1$. The length of the graph is also $N$, and $j != i$ is in the list $graph[i]$ exactly once, if and only if nodes i and j are connected.

Task is to find out the length of the shortest path that visits every node. We're free to choose our own start and stop locations and are allowed to revisit nodes multiple times and reuse edges as we may like.


## Discussion

I'm going to initiate a data structure for this problem. It's just *bad code* to use variables like *cover2* and such. So be a good programmer, and take the pain of initializing a class for your object, which will help you _tremendously_ for not just this problem - but in your actual job.

For the sake of explanation, I'm going to put a story (also helps you to retain the logic in your mind if you're preparing for interviews). So, imagine you have to visit all cities of the world. You have a journal. Every city is represented by the data structure `Node` in our code. Lets read through the intuition

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/d/d8/2011_Tour_de_France_map.png/593px-2011_Tour_de_France_map.png"/>

## Intuition

1. We use BFS to make sure we visited the shortest path first because DFS doesn't guarantee the path to be the shortest. It can make you wander off on some other path.
2. We maintain the distance to each node in a HashMap. 

**What the duck are bits doing here?**
As it turns out, life would have been really simple if we could just keep all the visited nodes in a set called `visited`. Well, *Ce La Vie!* - the judge gives you a TLE.

So in order to solve that, we turn our journal into a hashset, in which we maintain a list of 
cities visited so far - just like a bucket list! Cross off the city you visit. For example

<pre>
1 1 1 0 0 1 1  journal
0 1 2 3 4 5 6  index
</pre>

implies you have visited cities with id 0, 1, 2 and 5 and 6 - basically wherever the bit is set.

**Okay how does this help?**
Well, you can just perform an bitwise `OR` to cross off the city that you just visited. *(Think!)*


## Implementation

### Defining a `Node`

Note that since we have a custom data structure, it is important that we provide a `__hash__` method to python so that it doesn't complain about keys being unhashable.

```python
class Node(object):
    def __init__(self, nodeId, visitedSoFar):
        self.id = nodeId
        self.journal = visitedSoFar
    
    def __eq__(self, other):
        return self.id == other.id and self.journal == other.journal

    def __repr__(self):
        return "Node({}, {})".format(self.id, bin(self.journal)[2:])
    
    def __hash__(self):
        return hash((self.id, self.journal))
    
```

### `shortestPathLength` Method

The actual implementation now looks surprisingly simple now that we have took the pain of defining what a `Node` is.


```python
def shortestPathLength(self, graph):
    """
    :type graph: List[List[int]]
    :rtype: int
    """
    N = len(graph)
    # 1<<i represents nodes visitedSoFar to reach this node
    # when initializing, we don't know the best node to start 
    # our journey around the world with. So we add all
    # nodes to our queue aka travel journal !
    q = collections.deque(Node(i, 1<<i) for i in range(N))
    distanceToThisNode = collections.defaultdict(lambda :N*N)
    for i in range(N): 
        distanceToThisNode[Node(i, 1<<i)] = 0
    
    endJournal = (1 << N) - 1
    # when we have visited all nodes, this is how our journal 
    # aka visitedSoFar at that node would look like.
    
    while(q):
        node = q.popleft()
        
        dist = distanceToThisNode[node]
        
        if(node.journal == endJournal):
            return dist 
        
        neighbouring_cities = graph[node.id]
        
        for city in neighbouring_cities:
            newJournal = node.journal | (1 << city)
            # doing an OR operation with 1<<city essentially adds
            # this city to the journal. aka sets that nodeId to 1
            
            neighbour_node = Node(city, newJournal)
                
            if dist+1 < distanceToThisNode[neighbour_node]:
                distanceToThisNode[neighbour_node] = dist+1
                q.append(neighbour_node)
    return -1
        
        
```



.