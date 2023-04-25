public static int solve(int width, int height, int nbBlocks, String[] grid) {
    // Convert grid to a 2D array of integers
    int[][] puzzle = new int[height][width];
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            puzzle[i][j] = Character.getNumericValue(grid[i].charAt(j));
        }
    }

    // Initialize variables for the search
    int[] blocks = new int[nbBlocks];
    boolean[][] visited = new boolean[height][width];
    int[] result = new int[nbBlocks];

    // Fill the blocks array with the numbers of the blocks
    for (int i = 0; i < nbBlocks; i++) {
        blocks[i] = i;
    }

    // Perform the depth-first search
    dfs(0, blocks, visited, puzzle, result);

    // Return the first block in the result array
    return result[0];
}

private static boolean dfs(int depth, int[] blocks, boolean[][] visited, int[][] puzzle, int[] result) {
    // If all blocks have been visited, we have found a solution
    if (depth == blocks.length) {
        return true;
    }

    // Try removing each block in turn
    for (int i = 0; i < blocks.length; i++) {
        int block = blocks[i];

        // If the block has already been visited, skip it
        if (visited[block / puzzle[0].length][block % puzzle[0].length]) {
            continue;
        }

        // Try moving the block to the right until it falls off the grid
        int row = block / puzzle[0].length;
        int col = block % puzzle[0].length;
        while (col < puzzle[0].length && puzzle[row][col] == puzzle[row][block % puzzle[0].length]) {
            col++;
        }
        col--;

        // If the block does not collide with any other blocks, remove it and continue the search
        boolean collides = false;
        for (int j = block % puzzle[0].length; j <= col; j++) {
            if (visited[row][j]) {
                collides = true;
                break;
            }
        }
        if (!collides) {
            for (int j = block % puzzle[0].length; j <= col; j++) {
                visited[row][j] = true;
            }
            result[depth] = block;
            if (dfs(depth + 1, blocks, visited, puzzle, result)) {
                return true;
            }
            for (int j = block % puzzle[0].length; j <= col; j++) {
                visited[row][j] = false;
            }
        }
    }

    // If no solution was found, return false
    return false;
}
