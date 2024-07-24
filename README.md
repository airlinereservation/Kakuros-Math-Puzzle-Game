# Kakuros-Math-Puzzle-Game

#include <iostream>
#include <vector>

using namespace std;

// Cell structure to represent each cell in the grid
struct Cell {
    int value; // The value in the cell
    int sumConstraint; // The sum constraint for the cell
};

// Node structure for linked list
struct Node {
    pair<int, int> coordinates; // Coordinates of the cell
    // Pointer to the next node
    Node* next;
    Node* prev;
};

// Class to represent the Kakuro puzzle
class KakuroPuzzle {
private:
    vector<vector<Cell>> grid; // 2D grid to store the puzzle
    bool gameWon; // Flag to track if the game is won
    Node* emptyCellsHead; // Head of doubly linked list to store coordinates of empty cells

    // Function to check if the game is won
    bool isGameWon() {
        return (grid[1][1].value == 9 && grid[1][2].value == 7 && grid[2][1].value == 4 && grid[2][2].value == 2 &&
            grid[0][1].value == grid[1][1].value + grid[2][1].value &&
            grid[0][2].value == grid[1][2].value + grid[2][2].value &&
            grid[1][0].value == grid[1][1].value + grid[1][2].value &&
            grid[2][0].value == grid[2][1].value + grid[2][2].value);
    }

    bool isGameWon2() {
        int sum1 = grid[1][1].value + grid[2][1].value + grid[3][1].value;
        int sum2 = grid[1][2].value + grid[2][2].value + grid[3][2].value;
        int sum3 = grid[1][3].value + grid[2][3].value + grid[3][3].value;
        int sum4 = grid[1][1].value + grid[1][2].value + grid[1][3].value;
        int sum5 = grid[2][1].value + grid[2][2].value + grid[2][3].value;
        int sum6 = grid[3][1].value + grid[3][2].value + grid[3][3].value;

        return (grid[1][1].value == 9 && grid[1][2].value == 6 && grid[1][3].value == 4 &&
            grid[2][1].value == 7 && grid[2][2].value == 1 && grid[2][3].value == 3 &&
            grid[3][1].value == 4 && grid[3][2].value == 2 && grid[3][3].value == 1 &&
            grid[0][1].value == sum1 && grid[0][2].value == sum2 && grid[0][3].value == sum3 &&
            grid[1][0].value == sum4 && grid[2][0].value == sum5 && grid[3][0].value == sum6);
    }

    bool isGameWon3() {
        return (grid[1][1].value == 7 && grid[1][2].value == 1 && grid[2][1].value == 9 && grid[2][2].value == 3 &&
            grid[2][3].value == 8 && grid[3][2].value == 2 && grid[3][3].value == 9 && grid[3][4].value == 1 &&
            grid[4][3].value == 6 && grid[4][4].value == 3);
    }

    // Function to print the puzzle grid
    void printGrid() {
        
        cout << "Current Grid:" << endl;
        for (int i = 0; i < grid.size(); ++i) {
            cout << " +---+---+---+---+---+" << endl;
            for (int j = 0; j < grid[i].size(); ++j) {
                cout << "| ";
                if (grid[i][j].value == 0) {
                    cout << " "; // Print empty space
                }
                else {
                    cout << grid[i][j].value; // Print the value of each cell
                }
                cout << " ";
            }
            cout << "|" << endl;
        }
        cout << " +---+---+---+---+---+" << endl << endl;
    }

public:
    // Constructor to initialize the puzzle grid
    KakuroPuzzle(const vector<vector<int>>& puzzle) : gameWon(false), emptyCellsHead(nullptr) {
        grid.resize(puzzle.size(), vector<Cell>(puzzle[0].size()));

        for (int i = 0; i < puzzle.size(); ++i) {
            for (int j = 0; j < puzzle[i].size(); ++j) {
                grid[i][j].value = puzzle[i][j];
                grid[i][j].sumConstraint = 0; // Initialize sum constraint to 0

                // If the cell is empty, add its coordinates to the linked list
                if (grid[i][j].value == 0) {
                    Node* newNode = new Node();
                    newNode->coordinates = { i, j };
                    newNode->prev = nullptr;
                    newNode->next = emptyCellsHead;
                    if (emptyCellsHead != nullptr) {
                        emptyCellsHead->prev = newNode;
                    }
                    emptyCellsHead = newNode;
                }
            }
        }
    }

    // Function to fill the empty cells of the puzzle
    void fillEmptyCells(int x) {
        printGrid();
        Node* current = emptyCellsHead;
        while (current != nullptr) {
            pair<int, int> cell = current->coordinates;
            current = current->next;

            cout << "Enter value for cell [" << cell.first << "][" << cell.second << "]: ";
            cin >> grid[cell.first][cell.second].value;
            printGrid();

            if (x == 1 && isGameWon()) {
                cout << "\t ---------------------------------------------------------------------\n";
                cout << "\t | ***** Congratulations! You WON the Beginner Level GAME! :D  ***** |" << endl;
                cout << "\t ---------------------------------------------------------------------\n";
                gameWon = true;
                return;
            }
            else if (x == 2 && isGameWon2()) {
                cout << "\t ---------------------------------------------------------------------\n";
                cout << "\t | ***** Congratulations! You WON the Intermediate Level GAME! :D  ***** |" << endl;
                cout << "\t ---------------------------------------------------------------------\n";
                gameWon = true;
                return;
            }
        }

        cout << "\t --------------------------------------------\n";
        cout << "\t | ***** Sorry, YOU LOST the GAME :(  ***** |" << endl;
        cout << "\t --------------------------------------------\n";
        gameWon = false;
    }

    // Function to fill the empty cells of the puzzle for advanced level
    void fillEmptyCells3() {
        printGrid();

        // Define the coordinates of the cells to be filled
        vector<pair<int, int>> cellCoordinates = {
            {4, 4}, {4, 3}, {3, 4}, {3, 3}, {3, 2},
            {2, 3}, {2, 2}, {2, 1}, {1, 2}, {1, 1}
        };

        for (const auto& cell : cellCoordinates) {
            int row = cell.first;
            int col = cell.second;

            cout << "Enter value for cell [" << row << "][" << col << "]: ";
            cin >> grid[row][col].value;
            printGrid();

            if (isGameWon3()) {
                cout << "\t ---------------------------------------------------------------------\n";
                cout << "\t | ***** Congratulations! You WON the Advance Level GAME! :D  ***** |" << endl;
                cout << "\t ---------------------------------------------------------------------\n";
                gameWon = true;
                return;
            }
        }

        cout << "\t --------------------------------------------\n";
        cout << "\t | ***** Sorry, YOU LOST the GAME :(  ***** |" << endl;
        cout << "\t --------------------------------------------\n";
        gameWon = false;
    }

    // Destructor to free memory allocated for the linked list
    ~KakuroPuzzle() {
        Node* current = emptyCellsHead;
        while (current != nullptr) {
            Node* next = current->next;
            delete current;
            current = next;
        }
    }
};

#include <string>
#include <vector>
using namespace std;

int main() {
    string decision;
    int option;
    bool playedLevel = false;

    // Beginner level vector
    vector<vector<int>> beginnerGrid1 = {
        {0, 13, 9},
        {16, 0, 0},
        {6, 0, 0}
    };

    vector<vector<int>> beginnerGrid2 = {
        {0, 8, 7},
        {12, 0, 0},
        {3, 0, 0}
    };

    // Intermediate level vector
    vector<vector<int>> intermediateGrid1 = {
        {0, 20, 9, 8},
        {19, 0, 0, 0},
        {11, 0, 0, 0},
        {7, 0, 0, 0}
    };

    vector<vector<int>> intermediateGrid2 = {
        {0, 15, 11, 7},
        {20, 0, 0, 0},
        {7, 0, 0, 0},
        {6, 0, 0, 0}
    };

    // Advanced level vector
    vector<vector<int>> advanceGrid3 = {
        {0, 16, 6, 0, 0},
        {8, 0, 0, 23, 0},
        {20, 0, 0, 0, 4},
        {0, 12, 0, 0, 0},
        {0, 0, 9, 0, 0}
    };

    KakuroPuzzle puzzleAdvanced(advanceGrid3);

    do {
        cout << "\n\n\t******** Kakuro Math Puzzle Solver Game ********\n" << endl << endl;

        cout << "\n \t    Welcome To Kakuro Math Puzzle Solver Game \n\n";
        cout << "\t\t Choose the Level You like to Play \n\t \t   1. Beginner Level\n\t\t   2. Intermediate Level\n\t\t   3. Advanced Level\n\n";
        cin >> option;
        switch (option) {
        case 1:
            cout << "Please fill the empty cells of the puzzle (0 indicates empty cell):" << endl;
            if (!playedLevel) {
                KakuroPuzzle puzzleBeginner(beginnerGrid1);
                puzzleBeginner.fillEmptyCells(1);
                playedLevel = true;
            }
            else {
                KakuroPuzzle puzzleBeginner(beginnerGrid2);
                puzzleBeginner.fillEmptyCells(1);
            }
            break;
        case 2:
            cout << "Please fill the empty cells of the puzzle (0 indicates empty cell):" << endl;
            if (!playedLevel) {
                KakuroPuzzle puzzleIntermediate(intermediateGrid1);
                puzzleIntermediate.fillEmptyCells(2);
                playedLevel = true;
            }
            else {
                KakuroPuzzle puzzleIntermediate(intermediateGrid2);
                puzzleIntermediate.fillEmptyCells(2);
            }
            break;
        case 3:
            cout << "Please fill the empty cells of the puzzle (0 indicates empty cell):" << endl;
            puzzleAdvanced.fillEmptyCells3();
            break;
        default:
            cout << "     \n\t*****  The Option You Choose is Invalid  ******  \n\n";
            break;
        }
        cout << endl << "\t----------------------------------------------\n";
        cout << "\t|   Do You want to play the Game Again ???   |\n\t|   Enter \" y \" ( yes ) OR Exit the Game     |\n";
        cout << "\t----------------------------------------------\n\n";
        cin >> decision;
    } while (decision == "y" || decision == "yes");

    return 0;
}
