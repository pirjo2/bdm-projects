# BigData 2025 Template Repository

![TartuLogo](./images/logo_ut_0.png)

Project [Big Data](https://courses.cs.ut.ee/2025/bdm/spring/Main/HomePage) is provided by [University of Tartu](https://courses.cs.ut.ee/).

Students: Pirjo Vainjärv, Eidi Paas

## Data Sources
The data used for the analysis was provided in CSV format and includes details on flights, specifically:
- ORIGIN: The airport of departure.
- DEST: The airport of arrival.
- FL_DATE: The flight date.
- DISTANCE: The flight distance in miles.
- The dataset was loaded into Spark for analysis.

## Data Transformation
We performed data transformation and cleaning using Spark DataFrames, rows with null values were dropped.
We selected the columns ORIGIN, DEST, FL_DATE, and DISTANCE to focus on the necessary flight details for the graph construction.
Graph construction:
- Vertices (airports): Each unique airport is treated as a vertex.
- Edges (flights): Each flight between two airports is treated as an edge.
The graph is created using GraphFrames with airports as vertices and flights as edges.
After the transformation, there were a total of 296 airports and a total of 6429338 flights in the data.

## How to run

1.  **Start Docker Desktop App**:
    Ensure that the Docker Desktop application is running on your system.

2.  **Open Project Directory in Command Line**:
    Open your command line interface (e.g., Terminal on macOS/Linux, Command Prompt or PowerShell on Windows) and navigate to the project directory (`project_3`).

    ```bash
    cd project_3
    ```

3.  **Run Docker Compose**:
    Execute the following command to start the Docker containers in detached mode:

    ```bash
    docker compose up -d
    ```

4.  **Access the Application**:
    Once the containers are up and running, open your web browser and go to the following URL:

    ```
    http://localhost:8888/
    ```

5.  **Stop the Application**:
    To stop and remove the containers, execute the following command in the same directory:

    ```bash
    docker compose down
    ```

## Queries  NB! Please see pdf file of report for screenshots of the results
# Query 1 -  Compute different statistics : in-degree, out-degree, total degree and triangle
count
- In-degree: The number of flights arriving at an airport.
- Out-degree: The number of flights departing from an airport.
- Total Degree: The sum of in-degree and out-degree for each airport.
- Triangle Count: The number of triangles involving each airport.
First, we computed the in-degree by counting how many flights arrive at each airport (dst). Then, we calculated the out-degree by counting how many flights depart from each airport (src). Finally, we combined both to compute the total degree for each airport by summing in-degree and out-degree. Missing values were handled using coalesce.

Calculating the number of unique triangles in the graph by joining edges to form two-step paths (A → B → C) and then checking if a closing edge (C → A) exists. To avoid duplicate counting, we sorted and filtered nodes (A < B < C). 

# Query 2 - Compute the total number of triangles in the graph
The code calculates and displays the number of triangles each airport is part of in the graph. Each triangle is a set of three airports that are all interconnected by flights. The result was validated against triangleCount() function result. The result of Q2 was the total of triangles in the graph is 16015.

# Query 3 - Compute a centrality measure of your choice natively on Spark using Graphframes
Centrality measures help to identify the most important airports based on their position in the network. For this project, we selected degree centrality, which is computed by the total degree of each airport. We normalized the total degree by dividing it by the total number of airports in the data. The result was validated … As of the result the top 1st airport is ATL airport.

# Query 4 - Implement the PageRank algorithm natively on Spark using Graphframes
The code first implements the PageRank algorithm manually. It starts by assigning an initial rank of 1.0 to every airport in the graph. Then, it calculates how many outgoing connections each airport has. It simulates how each airport passes part of its rank to connected airports over 10 iterations. If an airport has no outgoing flights, its rank is evenly spread across all airports. The new rank of each airport is calculated using a damping factor of 0.85 to include both the received contributions and a small random teleportation factor. After the iterations, the ranks are scaled and normalized so that the total rank equals the number of airports.
After this, the same PageRank calculation is done using the built-in GraphFrames method. The damping factor and iteration count are kept the same. The results are also scaled and normalized to be comparable with the manual version. Finally, the airports are ranked by importance, and the top ten are displayed. As a result, the most important airport is ATL.

# Query 5 - Find the group of the most connected airports
The group of the most connected airports are connected via edges between each other. The algorithm logic, Breadth-First Search (BFS), was looked up from this source: https://cp-algorithms.com/graph /search-for-connected-components.html .
The dataset is cleaned by removing rows with missing origin or destination values. A list of flight connections is created, and reversed edges are added to make the graph undirected. A manual search starting from one airport is used to find all airports connected to it. This helps find the largest group of connected airports. This resulted in 1 big component, thus every airport is connected with some path to it.
The same process is repeated using GraphFrames. The built-in connectedComponents() function finds all groups of connected airports, and the largest group is selected based on size. This method approved of our finding of 1 big component, the picture shows only 10 of the airports in that component (eg. when you print it out, you will get all of the airports names).


## Note for Students

* Clone the created repository offline;
* Add your name and surname into the Readme file and your teammates as collaborators
* Complete the field above 
* Make any changes to your repository according to the specific assignment;
* Ensure code reproducibility and instructions on how to replicate the results;
* Add an open-source license, e.g., Apache 2.0;
* convert README in pdf
* keep one report for all projects

