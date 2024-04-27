# CDS-DS-210-Final-Project
>Angelo Amato
>
>1 May 2024
>
>Professor Kontothanassis

## About
The game Wikispeedia is an online game that involves navigating through the Wikipedia website towards a "target article" using only the hyperlinks on each  page. The player is supposed to do this in as little clicks, and as fast as possible. In this project, I will use the smaller(~4,600 articles) version of Wikipedia that the game uses. The Stanford Network Analysis Project (SNAP) has published a dataset of the links in this game.

In this project I am aiming to calculate the average number of clicks to navigate from one random page to another. In exploring the [Wikispeedia website](https://dlab.epfl.ch/wikispeedia/play/), the average number of clicks is about 4-10, with the record for some scenarios being even lower than that. I want to see if the records held for certain runs are only that high because people are missing hidden routes.  

## Code Description
The project is broken down into three files: `graph_read.rs`, `bfs.rs`, and `main.rs`. The first file `graph_read.rs`, is primarily used to create the graph from the tsv files and convert them to a usable format. The `bfs.rs` file contains the helper function in order to calculate the average distance from any given node to all other nodes. Finally, the `main.rs` file imports the functions and types from the other two files and runs the actual calculations. It also formats the output so all of the results are inline with the actual article name. 

Below is a breakdown of the code in each file:

#### `graph_read.rs`
This file houses helper functions that read the tsv files containing the data. There were two files: `articles.tsv`, which contained the names of the articles in alphabetical order, and `links.tsv`, which contained a list of links that form the edges of the directed graph. 

* `Graph`
  *  a struct representing a directed graph
  *  contains attributes n (the number of nodes) and an outedges (an adjacency list representing the graph)
  *  contains several methods including a `create_directed()` which creates a Graph struct from a List of Edges
 
* `create_key`
  * a helper function to create key to turn the names of articles into numeric values for the graph
  * takes a file path as input (assumes it is a tsv file
  * returns a HashMap, with the article names as keys and the corosponding numbers as values
  * values will range from 0 to n-1, with n being the number of articles

* `reverse_key`
  * a helper function that takes a HashMap key and retruns a Hashmap with the keys as values and vice versa
  * assumes that hte values are non-repeating numbers from 0 to n-1, with n being the number of articles
  * serves as a way to get the article names from the numeric values in the graph.
 
* `numbered_nodes_graph`
  * takes a HashMap key and a tsv file path as arguments
  * returns a graph representing the names articles in the tsv files
  * uses the HashMap key to change the string names of the articles to numerics
  * assumes the tsv file is two columns, representing the starting and ending nodes of a list of edges, with each new line being a different edge

#### `bfs.rs`
This file contains the function that runs the breadth first search algorithm used to calculate the average distance from a given node to all  *connected* nodes in the graph.

* `get_average_distance`
  * takes a starting vertex and a graph and calculates the distance to all other connected nodes in the graph
  * gets the distance to every reachable node in a vector, then gets the average of all values in the vector
  * does not take into account vectors that may not be connected

#### `main.rs`
This file calls some of the helper functions listed above and calculates the average distance for all starting nodes in the graph. It also prints the outputs as all of the articles with their average distance to all other nodes.

* `decode_names`
  * takes the percent encoded name of the article (which is how the data is originally formated)
  * changes the article names to ones with special characters
  * also replaces underscores with spaces
    
* `main`
  * creates a key and and a reverse key from the `articles.tsv` file
  * uses the key and the `links.tsv` file to make a graph of the Wikipedia articles
  * Prints headers for the output
  * Iterates through each of the nodes in the graph
      * Gets the average distance to all other connected articles and adds it to a vector
      * Uses the reverse key to get the article name
      * Changes all percent encodings to special characters
      * Prints the article name with the average distance to all other connected articles
  * Uses the vector and finds the average of the average distances, the maximum, and the minimum
  

#### Tests
* Test 1
  * Tests the `get_average_distance` function with a 4 node graph of nodes all in a line
  * Gets the average distance of all nodes to the 0 node (should be 2.0)
  * <img width="445" alt="image" src="https://github.com/Angelo-2231/CDS-DS-210-Final-Project/assets/64280204/22e6c88b-ced8-48d5-b4ec-b86761c67ace">

 
* Test 2
  * Tests the `get_average_distance` function with a slightly more complex graph with edges in differing directions
  * Gets the average distance of all nodes to the 0 node (should be 1.75)
  * <img width="445" alt="image" src="https://github.com/Angelo-2231/CDS-DS-210-Final-Project/assets/64280204/c9fa9881-18a8-42a8-84ba-8906661a3e69">

* Test 3
  * Tests the `reverse_key` function
  * makes sure that the function properly switches the keys and values (assuming no repeated keys) 


## Output and Observations

## Sources
1. [Stanford Network Analysis Project (SNAP) Dataset](https://snap.stanford.edu/data/wikispeedia.html)
2. [Wikispeedia](https://dlab.epfl.ch/wikispeedia/play/)
3. [ChatGPT Conversation 1](https://chat.openai.com/share/960d84a6-8bd0-435a-9f49-1ed69f4ae4a4)
4. [ChatGPT Conversation 2](https://chat.openai.com/share/d093b3ba-81fd-48b3-a603-1fa45248bf54)
