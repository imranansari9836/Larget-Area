# Larget-Area
import java.io.*;
import java.math.*;
import java.security.*;
import java.text.*;
import java.util.*;
import java.util.concurrent.*;
import java.util.function.*;
import java.util.regex.*;
import java.util.stream.*;
import static java.util.stream.Collectors.joining;
import static java.util.stream.Collectors.toList;


class Result {

    /*
     * Complete the 'getMaxArea' function below.
     *
     * The function is expected to return a LONG_INTEGER_ARRAY.
     * The function accepts following parameters:
     *  1. INTEGER w
     *  2. INTEGER h
     *  3. BOOLEAN_ARRAY isVertical
     *  4. INTEGER_ARRAY distance
     */

    public static List<Long> getMaxArea(int w, int h, List<Boolean> isVertical, List<Integer> distance) {
    List<Long> maxAreas = new ArrayList<>();

        List<Integer> verticalLines = new ArrayList<>();
        verticalLines.add(0); verticalLines.add(w);
        List<Integer> horizontalLines = new ArrayList<>();
        horizontalLines.add(0); horizontalLines.add(h);

        int noOfLines = isVertical.size();

        int maxHeight = h;
        int maxWidth = w;

        for (int i=0; i<noOfLines; i++) {
            if (isVertical.get(i)) {
                insert(verticalLines, distance.get(i));
                maxWidth = findMaxDistance(verticalLines);
            } else {
                insert(horizontalLines, distance.get(i));
                maxHeight = findMaxDistance(horizontalLines);
            }
            Long area = ((long) maxHeight * maxWidth);
            maxAreas.add(area);
        }

        return maxAreas;
    }

    private static int findMaxDistance(List<Integer> lines) {
        int maxDistance = Integer.MIN_VALUE;
        int i = lines.size()-1;
        while (i > 0) {
            maxDistance = Math.max(maxDistance, lines.get(i) - lines.get(i-1));
            i--;
        }
        return maxDistance;
    }

    private static void insert(List<Integer> lines, Integer value) {
        lines.add(value);
        int i = lines.size()-1;
        while (i > 0 && lines.get(i-1) > lines.get(i)) {
            Integer temp = lines.get(i-1);
            lines.set(i-1, lines.get(i));
            lines.set(i, temp);
            i--;
        }
    }

    public static void main(String[] args) {
        System.out.println(getMaxArea(4,4, Arrays.asList(false,true),Arrays.asList(3,1)));//12 9
        System.out.println(getMaxArea(2,2,Arrays.asList(false,true),Arrays.asList(1,1)));//2 1
        System.out.println(getMaxArea(4,3,Arrays.asList(true,true),Arrays.asList(1,3)));//9 6
    }
}

public class Solution {
    public static void main(String[] args) throws IOException {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

        int w = Integer.parseInt(bufferedReader.readLine().trim());

        int h = Integer.parseInt(bufferedReader.readLine().trim());

        int isVerticalCount = Integer.parseInt(bufferedReader.readLine().trim());

        List<Boolean> isVertical = IntStream.range(0, isVerticalCount).mapToObj(i -> {
            try {
                return bufferedReader.readLine().replaceAll("\\s+$", "");
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .map(String::trim)
            .map(e -> Integer.parseInt(e) != 0)
            .collect(toList());

        int distanceCount = Integer.parseInt(bufferedReader.readLine().trim());

        List<Integer> distance = IntStream.range(0, distanceCount).mapToObj(i -> {
            try {
                return bufferedReader.readLine().replaceAll("\\s+$", "");
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        })
            .map(String::trim)
            .map(Integer::parseInt)
            .collect(toList());

        List<Long> result = Result.getMaxArea(w, h, isVertical, distance);

        bufferedWriter.write(
            result.stream()
                .map(Object::toString)
                .collect(joining("\n"))
            + "\n"
        );

        bufferedReader.close();
        bufferedWriter.close();
    }
}
