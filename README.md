# A* Pathfinding Algorithm Demo

This repository contains a simple web application that demonstrates the A* pathfinding algorithm using JavaScript and HTML. The application allows users to interactively set obstacles, start point, and end point on a grid. The A* algorithm is then applied to find the optimal path through the grid while avoiding obstacles.

## How to Use

1. **Left-click**: Toggle obstacles on the grid.
2. **Right-click**: Set the start point (green). 
3. **Drag Right Mouse Button**: Set the end point (red).

## Demo

You can try the live demo [here](https://adamstirtan.github.io/astar-js/).

## Implementation

The A* algorithm is implemented in JavaScript and the HTML canvas element is used to visualize the grid and the algorithm in action. The core components of the algorithm, such as the open set, closed set, and the heuristic function, are included in the JavaScript code.

## Code Snippet

Here is a snippet of the A* algorithm implementation:

```javascript
// A* pathfinding algorithm
function astar(start, end) {
    const openSet = [];
    const closedSet = [];
    openSet.push(start);

    while (openSet.length > 0) {
        // Find the node with the lowest total cost in the open set
        let currentNode = openSet[0];
        for (let i = 1; i < openSet.length; i++) {
            if (openSet[i].f < currentNode.f || (openSet[i].f === currentNode.f && openSet[i].h < currentNode.h)) {
                currentNode = openSet[i];
            }
        }

        // Remove the current node from the open set
        openSet.splice(openSet.indexOf(currentNode), 1);
        closedSet.push(currentNode);

        // If the current node is the goal, reconstruct the path
        if (currentNode.row === end.row && currentNode.col === end.col) {
            let path = [];
            let temp = currentNode;
            while (temp) {
                path.push(temp);
                temp = temp.parent;
            }
            return path.reverse();
        }

        // Generate neighbors of the current node
        const neighbors = [];
        const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]]; // Right, Down, Left, Up

        for (let dir of directions) {
            const neighborRow = currentNode.row + dir[0];
            const neighborCol = currentNode.col + dir[1];

            if (isValidCell(neighborRow, neighborCol)) {
                const neighbor = {
                    row: neighborRow,
                    col: neighborCol,
                    g: currentNode.g + 1, // Cost to move to a neighboring cell is 1
                    h: heuristic({ row: neighborRow, col: neighborCol }, end),
                    f: 0,
                    parent: currentNode,
                };

                neighbor.f = neighbor.g + neighbor.h;

                // Check if the neighbor is already in the closed set
                if (closedSet.some((node) => node.row === neighbor.row && node.col === neighbor.col)) {
                    continue;
                }

                // Check if the neighbor is already in the open set
                const openSetNode = openSet.find((node) => node.row === neighbor.row && node.col === neighbor.col);
                if (!openSetNode || neighbor.g < openSetNode.g) {
                    openSet.push(neighbor);
                }
            }
        }
}

// Helper function to calculate the heuristic (Manhattan distance)
function heuristic(a, b) {
    return Math.abs(a.row - b.row) + Math.abs(a.col - b.col);
}

// Helper function to check if a cell is valid and not an obstacle
function isValidCell(row, col) {
    // Implementation details here
}

// ... (Other code)
