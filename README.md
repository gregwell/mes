# FEM simulations

This app allows you to create 2D transient state thermal process simulations using finite element method.

### Thermal problem

Thermal phenomena occurring in the transient state are described by the Fourier equation in the
following form:

![images/Untitled.png](images/Untitled.png)

The general solution can be described by the following system of equations:

![images/Untitled%201.png](images/Untitled%201.png)

The matrices in the equation are as follows:

![images/Untitled%202.png](images/Untitled%202.png)

![images/Untitled%203.png](images/Untitled%203.png)

The vector **t0** is known and contains the initial temperatures at each node. The vector **t1** contains the temperatures at each node after chosen step time. We calculate it for each iteration, then the values of **t1** vector are assigned to **t0** vector and the operation is repeated.

The longest correct step time can be calculated from the following equation:

![images/Untitled%204.png](images/Untitled%204.png)

n - the shortest side length of the element

k - thermal conductivity

ρ - density

Cp- specific heat

If more than one material is considered calculate a result for both and take the smaller one. For validation, divide the result by half and compare the calculations. Divide in half until they are the same (or almost the same.).

### Application flow

1. Read data from file.
2. Assign the coordinates to the nodes.
3. Handle boundary condition - assign BC flags to the nodes on the edges.
4. Assign the nodes to the elements.
5. Assign the physical properties to the elements.
6. Execute the thermal process simulation - repeat as many times as the file indicates
    1. Calculate H and C matrices.
    2. Calculate HBC matrix, add it to the H matrix and calculate P vector.
    3. Aggregate local matrices (creation of global matrices).
    4. Calculate Replacement H.
    5. Calculate Replacement P.
    6. Solve the system of equations.
    7. Print nodes temperatures and the maximum and minimum temperatures in the iteration.
    8. Assign t1 vector values to t0 vector.
    9. Repeat

### Input file

The file must be named **data.txt** and contain the following properties: (one line, separated by space):

- mesh width
- mesh height
- number of nodes horizontally
- number of nodes vertically
- order of integration - number of points to use when executing Gaussian Integration (2,3 or 4)
- physical properties for the two materials separately, sequentially; (firstly-external material)
    1. thermal conductivity
    2. density
    3. specific heat
- convective heat transfer coefficient
- ambient temperature
- initial temperature
- simulation time
- simulation step time

**Sample input file:**

> 0.06 0.22 13 45 4 0.8 2400 840 0.45789 916 3580 10 -32 6 36000 60

## Sample problem

### Description

A glass bottle of 40% vodka kept in a refrigerator at 6 degrees was put into a freezer at -32 degrees. Calculate when the vodka freezes (according to [this article](https://wersjatestowa.eu/w-jakich-temperaturach-zamarza-roztwor-etanolu/), a 40% ethanol solution freezes at -23 degrees). Nodes selected for analysis:

![images/Untitled%205.png](images/Untitled%205.png)

### Result:

The result shows temperatures in each selected node after each 60 second step time. 

The temperatures at each mesh node will reach a temperature of -23 degrees or lower after exactly 4 hours and 10 minutes. Console output:

![images/Untitled%206.png](images/Untitled%206.png)
