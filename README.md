import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class CSVProcessor {

    public static List<String> processCSVMessage(List<String> csvMessage) {
        // Extract the header without line separators
        String originalHeader = csvMessage.get(0);
        String header = originalHeader.replaceAll("\\r?\\n", "");

        // Find the index of the first line separator in the header
        int headerSeparatorIndex = originalHeader.indexOf(System.lineSeparator());

        // Check if the header contains a line separator
        if (headerSeparatorIndex != -1) {
            // Replace the line separator with ",UUID" and another line separator
            header = originalHeader.substring(0, headerSeparatorIndex) + ",UUID" + System.lineSeparator() + originalHeader.substring(headerSeparatorIndex + 1);
        } else {
            // If the header does not contain a line separator, simply append it with ",UUID" and a line separator
            header = header + ",UUID" + System.lineSeparator();
        }

        // Initialize an empty list to store processed rows
        List<String> processedRows = new ArrayList<>();

        // Traverse from the 1st row till the end of csvMessage
        for (int i = 1; i < csvMessage.size(); i++) {
            String row = csvMessage.get(i);

            // Find the index of the first line separator in the row
            int rowSeparatorIndex = row.indexOf(System.lineSeparator());

            // Check if the row contains a line separator
            if (rowSeparatorIndex != -1) {
                // Replace the line separator with ",UUID" and another line separator
                String modifiedRow = row.substring(0, rowSeparatorIndex) + ",UUID" + System.lineSeparator() + row.substring(rowSeparatorIndex + 1);
                processedRows.add(modifiedRow);
            } else {
                // If the row does not contain a line separator, simply append it with ",UUID" and a line separator
                processedRows.add(row + ",UUID" + System.lineSeparator());
            }
        }

        // Create a new list and add the header with UUID, and processed rows
        List<String> resultList = new ArrayList<>();
        resultList.add(header);
        resultList.addAll(processedRows);

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
