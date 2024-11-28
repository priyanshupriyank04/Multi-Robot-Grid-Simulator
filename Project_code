#include <iostream>
#include <cstdlib>   //used for random number generation & memory management
#include <ctime>     //to ensure the seeding of random number generation is always different
#include <algorithm> // for random_shuffle() fn  to shuffle directions

using namespace std;

// initialising Grid class which includes all data members and method functions related to the grid
class Grid
{
private:
    int rows, cols; // represents the grid dimensions
    int **grid;     // create a 2d dynamic array which stores the grid state whether visited/unvisited

    int visitedcount; // number of cells visited so far

public:
    // constructor function to initialise the grid with user specified dimensions
    Grid(int r, int c)
    {
        rows = r;
        cols = c;
        visitedcount = 0;

        // dynamically allocate memory for the grid
        grid = new int *[rows];
        // use nested for loops to iterate through whole grid and initialise the grid as unvisited for all cells
        for (int i = 0; i < rows; i++)
        {
            grid[i] = new int[cols];
            for (int j = 0; j < cols; j++)
            {
                grid[i][j] = 0; // all cells being initialised as unvisited(0) one by one
            }
        }
    }

    ~Grid()
    {
        for (int i = 0; i < rows; i++)
        {
            delete[] grid[i]; // dynamically free each row
        }

        delete[] grid; // dynamically free the array of pointers
    }

    // bool function to check if a certain grid cell[x][y] is visited
    bool isVisited(int x, int y)
    {
        return grid[x][y] == 1; // returns true if cell is visited
    }

    // function to mark a specific cell[x][y] as visited
    void markVisited(int x, int y)
    {
        if (!isVisited(x, y)) // check if current is visited before marking visited
        {
            grid[x][y] = 1; // mark the cell visited
            visitedcount++; // increase the number of cells visited
        }
    }

    // check if all the cells are visited or not
    bool isComplete()
    {
        return visitedcount == rows * cols; // returns true if visitedcount is equal to number of cells
    }

    // function to display the current state of the grid
    void display()
    {
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
            {
                if (grid[i][j] == 1)
                {
                    cout << " X "; // display X for visited cell
                }
                else
                {
                    cout << " . "; // display . for unvisited cell
                }
            }
            cout << endl;
        }
    }

    // getter fn for the number of rows
    int getRows()
    {
        return rows; // allows the robot class to get the number of rows and columns without interacting with grid class and saves time
    }

    // getter fn for the number of columns
    int getCols()
    {
        return cols;
    }
};

// Robot class which contains all data members and functions related to robots
class Robot
{
private:
    int x, y;    // for storing current pos of robot
    Grid &grid1; // reference to the grid1 object the robot is going to work on

    // function to check whether moving to (newx,newy) is valid or not
    bool isValidMove(int newx, int newy)
    {
        return newx >= 0 && newx < grid1.getRows() && newy >= 0 && newy < grid1.getCols() && !grid1.isVisited(newx, newy);
    }
    // above function returns true only if newx,newy are within perimissible bounds and also the position is unvisited

public:
    // constructor fn to initialise the robot at a random starting position
    Robot(Grid &g, int startX, int startY) : grid1(g)
    {
        grid1 = g;               // initialise the grid reference
        x = startX;              // set starting x coord
        y = startY;              // set starting y coord
        grid1.markVisited(x, y); // mark the starting position as visited
    }

    // fn to move the robot to an adjacent unvisited cell
    void move()
{
    // Directions -> up, down, left, right
    static const int dx[] = {-1, 1, 0, 0};
    static const int dy[] = {0, 0, -1, 1};

    // Array of directions
    int directions[] = {0, 1, 2, 3};
    random_shuffle(directions, directions + 4); // Shuffle directions randomly

    // Try up to 4 shuffled directions to find a possible valid move
    for (int i = 0; i < 4; i++)
    {
        int dir = directions[i];
        int newx = x + dx[dir];
        int newy = y + dy[dir];

        // If valid move, update the robot's current position and mark the cell as visited
        if (isValidMove(newx, newy))
        {
            x = newx;
            y = newy;
            grid1.markVisited(x, y);
            return;
        }
    }

    // If no valid moves, perform a forced scan for the nearest unvisited cell
    for (int nx = 0; nx < grid1.getRows(); nx++)
    {
        for (int ny = 0; ny < grid1.getCols(); ny++)
        {
            if (!grid1.isVisited(nx, ny))
            {
                x = nx;
                y = ny;
                grid1.markVisited(x, y);
                return;
            }
        }
    }
}

};

// now the main() function to run the user simulation
int main()
{
    srand(time(0)); // seeding the random number generator to ensure we get different position of robots and movements at each runtime

    // Step1 -> initialise the grid, for now we have taken fixed input as rows = 5 and cols = 5
    int rows = 5;
    int cols = 5;
    Grid grid(rows, cols); // creates a 5x5 2d grid

    // Step2 -> Create multiple robots, again this can be fed user input but for now no. of robots fixed = 3
    int robotCount = 3;
    Robot *robots[robotCount]; // array to store objects of Robot class type

    // initialise all these robots at random position in the 2d grid

    for (int i = 0; i < robotCount; i++)
    {
        int startX = rand() % rows; // initialise random X coord
        int startY = rand() % cols; // initialise random Y coord
        robots[i] = new Robot(grid, startX, startY);
    }

    // Step3 -> Simulation loop
    while (!grid.isComplete())
    {
        for (int i = 0; i < robotCount; i++)
        {
            robots[i]->move(); // run the move function for each robot until grid is completely visited
        }

        // Display the grid after all robots move once

        // system("clear"); // if you want to print only one grid and keep showing changes in the same code
        grid.display();
        cout << endl;

        // to slow down the simulation with gap of 1 second between each display
        system("sleep 1");
    }

    // Step4 -> cleanup by dynamic memory deallocation
    for (int i = 0; i < robotCount; i++)
    {
        delete robots[i]; // deallocated memory for each robot
    }

    // Step 5-> Simulation start
    cout << "All grid cells have been visited" << endl;

    return 0;
}
