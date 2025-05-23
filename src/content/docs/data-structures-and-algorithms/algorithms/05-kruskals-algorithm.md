---
title: Kruskal's Algorithm
sidebar:
  order: 5
slug: data-structures-and-algorithms/algorithms/kruskals-algorithm
prev: true
next: true
---

A [greedy algorithm](/data-structures-and-algorithms/algorithms/introduction#greedy-algorithm) to find MST of a graph. Sorts all the edges of the graph by their weights and adds them one by one to
the MST, ensuring that no cycles are formed.

## Implementation

```java
import java.util.*;

class Edge implements Comparable<Edge> {
    int src, dest, weight;

    public Edge(int src, int dest, int weight) {
        this.src = src;
        this.dest = dest;
        this.weight = weight;
    }

    public int compareTo(Edge compareEdge) {
        return this.weight - compareEdge.weight;
    }
}

class Subset {
    int parent, rank;
}

public class KruskalMST {
    int V, E;
    Edge[] edges;

    KruskalMST(int v, int e) {
        V = v;
        E = e;
        edges = new Edge[E];
        for (int i = 0; i < e; ++i) {
            edges[i] = new Edge(0, 0, 0);
        }
    }

    int find(Subset[] subsets, int i) {
        if (subsets[i].parent != i) {
            subsets[i].parent = find(subsets, subsets[i].parent);
        }
        return subsets[i].parent;
    }

    void union(Subset[] subsets, int x, int y) {
        int xroot = find(subsets, x);
        int yroot = find(subsets, y);

        if (subsets[xroot].rank < subsets[yroot].rank) {
            subsets[xroot].parent = yroot;
        } else if (subsets[xroot].rank > subsets[yroot].rank) {
            subsets[yroot].parent = xroot;
        } else {
            subsets[yroot].parent = xroot;
            subsets[xroot].rank++;
        }
    }

    void kruskalMST() {
        Edge[] result = new Edge[V];
        int e = 0;
        int i = 0;
        for (i = 0; i < V; ++i) {
            result[i] = new Edge(0, 0, 0);
        }

        Arrays.sort(edges);

        Subset[] subsets = new Subset[V];
        for (i = 0; i < V; ++i) {
            subsets[i] = new Subset();
        }

        for (int v = 0; v < V; ++v) {
            subsets[v].parent = v;
            subsets[v].rank = 0;
        }

        i = 0;

        while (e < V - 1) {
            Edge nextEdge = edges[i++];

            int x = find(subsets, nextEdge.src);
            int y = find(subsets, nextEdge.dest);

            if (x != y) {
                result[e++] = nextEdge;
                union(subsets, x, y);
            }
        }

        System.out.println("Following are the edges in the constructed MST");
        for (i = 0; i < e; ++i) {
            System.out.println(result[i].src + " -- " + result[i].dest + " == " + result[i].weight);
        }
    }

    public static void main(String[] args) {
        int V = 6;
        int E = 8;
        KruskalMST graph = new KruskalMST(V, E);

        graph.edges[0] = new Edge(0, 1, 1);
        graph.edges[1] = new Edge(1, 2, 2);
        graph.edges[2] = new Edge(0, 4, 3);
        graph.edges[3] = new Edge(0, 3, 4);
        graph.edges[4] = new Edge(1, 4, 5);
        graph.edges[5] = new Edge(2, 5, 6);
        graph.edges[6] = new Edge(3, 4, 7);
        graph.edges[7] = new Edge(4, 5, 8);

        graph.kruskalMST();
    }
}
```

This code demonstrates how to find the MST of a graph using Kruskal's Algorithm.
The `Edge` class represents an edge in the graph, and the `KruskalMST` class
contains the logic for finding the MST.

## Example

```txt
    1       2
A-------B-------C
| \     |      /|
|  \    |     / |
4   3   5    6  |
|    \  |  /    |
|     \ | /     |
D-------E-------F
    7       8
```

1. Sort the edges by weight:

   - (A-B, 1), (B-C, 2), (A-E, 3), (A-D, 4), (B-E, 5), (C-F, 6), (D-E, 7),
     (E-F, 8)

2. Add edges to the MST, ensuring no cycles are formed:
   - Add (A-B, 1)
   - Add (B-C, 2)
   - Add (A-E, 3)
   - Add (A-D, 4)
   - Add (C-F, 6)

The resulting MST will have the edges: (A-B, 1), (B-C, 2), (A-E, 3), (A-D, 4),
(C-F, 6).
