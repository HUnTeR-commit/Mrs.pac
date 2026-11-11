import java.util.*;

public class PacmanMazeGenerator {
    private static final int PATH = 0, WALL = 1, GHOST = 2, TELEPORT = 3;
    private final int width, height;
    private final int[][] grid;
    private final Random rand = new Random();

    public PacmanMazeGenerator(int width, int height) {
        this.width = (width % 2 == 0) ? width + 1 : width;
        this.height = (height % 2 == 0) ? height + 1 : height;
        this.grid = new int[this.height][this.width];
    }

    public void generate() {
        for (int[] row : grid){
            Arrays.fill(row, WALL);
    }
        carvePaths(1, 1);

        createLoops();
        addTeleporters();
        addGhostSpawn();
    }

    private void carvePaths(int x, int y) {
        grid[y][x] = PATH;

        List<int[]> dirs = Arrays.asList(new int[]{0, -2}, new int[]{0, 2}, new int[]{-2, 0}, new int[]{2, 0});
        Collections.shuffle(dirs, rand);

        for (int[] d : dirs) {
            int nx = x + d[0], ny = y + d[1];
            if (nx > 0 && nx < width - 1 && ny > 0 && ny < height - 1 && grid[ny][nx] == WALL) {
                grid[y + d[1] / 2][x + d[0] / 2] = PATH;
                carvePaths(nx, ny);
            }
        }
    }

    private void createLoops() {
        int cy = height / 2, cx = width / 2;
        for (int y = 1; y < height - 1; y++) {
            for (int x = 1; x < width - 1; x++) {
                if (Math.abs(y - cy) <= 2 && Math.abs(x - cx) <= 3) {
                    continue;
                }
                if (grid[y][x] == WALL && countAdjacentPaths(x, y) >= 2 && rand.nextDouble() < 0.25) {
                    grid[y][x] = PATH;
                }
            }
        }
    }

    private int countAdjacentPaths(int x, int y) {
        int count = 0;
        if (grid[y - 1][x] == PATH) {
            count++;
        }
        if (grid[y + 1][x] == PATH){
            count++;
        }
        if (grid[y][x - 1] == PATH){
            count++;
        }
        if (grid[y][x + 1] == PATH){
            count++;
        }
        return count;
    }

    private void addGhostSpawn() {
        int cy = height / 2, cx = width / 2;
        for (int y = cy - 2; y <= cy + 2; y++) {
            for (int x = cx - 3; x <= cx + 3; x++) {
                boolean isBorder = (y == cy - 2 || y == cy + 2 || x == cx - 3 || x == cx + 3);
                grid[y][x] = isBorder ? WALL : GHOST;
            }
        }
        grid[cy - 2][cx] = PATH;
        if (cy - 3 > 0) grid[cy - 3][cx] = PATH;
    }

    private void addTeleporters() {
        int cy = height / 2;
        for (int x = 1; x < 3; x++) {
            grid[cy][x] = PATH;
            grid[cy][width - 1 - x] = PATH;
        }
        grid[cy][0] = TELEPORT;
        grid[cy][width - 1] = TELEPORT;
    }

    public void printMaze() {
        String[] symbols = {"  ", "██", "G ", "T "};
        for (int[] row : grid) {
            for (int cell : row) System.out.print(symbols[cell]);
            System.out.println();
        }
    }

    public static void main(String[] args) {
        PacmanMazeGenerator maze = new PacmanMazeGenerator(21, 21);
        maze.generate();
        maze.printMaze();
    }
}
