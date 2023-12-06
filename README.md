import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.UUID;

public class CSVProcessor {

    public static List<String> processCSVMessage(List<String> csvMessage) {
        // Take the 0th row until the first line separator
        String[] headerParts = csvMessage.get(0).split(System.lineSeparator());
        String header = headerParts[0];

        // Find the index of the first line separator in the header
        int separatorIndex = header.indexOf(System.lineSeparator());

        // Add ",UUID" after the first line separator and another line separator
        String modifiedHeader = header.substring(0, separatorIndex) + ",UUID" + System.lineSeparator() + header.substring(separatorIndex + 1);

        // Add ",UUID" and a line separator to the modified header
        String headerWithUUID = modifiedHeader + ",UUID" + System.lineSeparator();

        // Initialize an empty string to store processed rows
        StringBuilder processedRows = new StringBuilder();

        // Traverse from the 1st row till the end of csvMessage
        for (int i = 1; i < csvMessage.size(); i++) {
            String row = csvMessage.get(i);

            // Check if the row contains a line separator
            if (row.contains(System.lineSeparator())) {
                // Find the index of the first line separator in the row
                int rowSeparatorIndex = row.indexOf(System.lineSeparator());

                // Add ",UUID" after the first line separator and another line separator
                String modifiedRow = row.substring(0, rowSeparatorIndex) + ",UUID" + System.lineSeparator() + row.substring(rowSeparatorIndex + 1);

                // Add each modified row to the string
                processedRows.append(modifiedRow).append(",").append(UUID.randomUUID().toString().substring(0, 8))
                        .append(System.lineSeparator());
            } else {
                // If the row does not contain a line separator, simply append it with ",UUID" and a line separator
                processedRows.append(row).append(",UUID").append(System.lineSeparator());
            }
        }

        // Create a new list and add the header with UUID, and processed rows
        List<String> resultList = new ArrayList<>(Arrays.asList((headerWithUUID + processedRows.toString()).split(System.lineSeparator())));

        return resultList;
    }

    public static void main(String[] args) {
        // Example usage:
        List<String> csvMessage = new ArrayList<>();
        csvMessage.add("Header1,Header2,Header3\n");
        csvMessage.add("Data1,Data2,Data3\n");
        csvMessage.add("Data4,Data5,Data6\n");

        List<String> result = processCSVMessage(csvMessage);

        // Print the result
        for (String line : result) {
            System.out.print(line);
        }
    }
}
