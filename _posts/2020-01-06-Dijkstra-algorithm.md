---
published: ture
layout: post
title: "Dijkstra's algorithm"
author: Yu
categories: HowTo
tags:
- Dijkstra
- MATLAB
---


> If debugging is the process of removing bugs, then programming must be the process of putting them in.
>
> ---Edsger W. Dijkstra (cannot find the citation, this may be fabricated)

Dijkstra's algorithm is an algorithm for finding the shortest paths between nodes in a graph, which may represent, for example, road networks. 
The more common algorithm fixes a single node as the initial node and finds shortest paths from the initial node to all other nodes in the graph, producing a shortest-path tree.

The steps for implementing Dijkstra's algorithm are as follows:

- find the initial distance vector $$D$$ of each nodes (the distance from initial node to other nodes)
- find the minimum distance between the initial node and one other node, set the distance to vector $$D$$
- update the initial node index, from the new initial node find the minimum distance again.


Here is the MATLAB code for calculate the distance from a graph, the output vector is the shortest paths from initial node to other nodes.

![graph](https://i.imgur.com/IGrz8Oo.png)

```matlab
function dijkstra(V,o)
%V is adjacency matrix, o is the initial node index
A=V;
[m,n]=size(A);
d=zeros(1,m);% vector to store the distance of initial node and other nodes
d(:)=inf;
Q=A(o,:);%find the distance between two nodes (the initial node and others)
d(o)=0;% set the distance of start point to initial node is zero
p=10;%loop flag
while(min(Q)~=inf&p~=0)% stop loop when all distance is Inf or loop flag is zero
    [a id]=min(Q);% find the minimum distance and the index of the node
    Q(id)=inf;%set the minimum index to Inf 
    A(o,id)=inf;%set the minimum distance to Ind
    o=id;%update the initial node
    if d(id)>a
        d(id)=a;
    end
    for j=1:m%change the distance of each node
        if (Q(j)>d(id)+A(id,j))
            Q(j)=d(id)+A(id,j);
        end
    end
    p=0;%loop flat == 0
    for i=1:m%check inf value, set the loop flag > 0 (looping)
        if d(i)==inf
            p=p+1;
        end
    end
end
d
end
```

The distance from initial `node 1` to `node 2` is Inf, from `node 2` to `node 3` is 10, from `node 3` to `node 4` is 50, from `node 4` to `node 5` is 20, from `node 5` to `node 6` is 60. 

```matlab
V=[0 inf 10 inf 30 100;
inf 0 5 inf inf inf;
inf inf 0 50 inf inf;
inf inf inf 0 inf 10;
inf inf inf 20 0 60;
inf inf inf inf inf 0];


octave:3> dijkstra(V,1)
warning: Matlab-style short-circuit operation performed for operator &
warning: called from
    dijkstra at line 25 column 9
d =

     0   Inf    10    50    30    60

```
