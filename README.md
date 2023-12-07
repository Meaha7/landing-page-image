import java.util.ArrayList;
import java.util.List;
import java.util.UUID;

public class CSVProcessor {

    public static List<String> processCSVMessage(List<String> csvMessage) {
        // Extract the header without line separators
        String originalHeader = csvMessage.get(0);
        String[] headerParts = originalHeader.split("\\r?\\n");
        String header = String.join("", headerParts);

        // Add ",UUID" after the header and a line separator
        String headerWithUUID = header + ",UUID" + System.lineSeparator();

        // Initialize an empty list to store processed rows
        List<String> resultList = new ArrayList<>();
        resultList.add(headerWithUUID);

        // Traverse from the 2nd row till the end of csvMessage
        for (int i = 1; i < csvMessage.size(); i++) {
            String row = csvMessage.get(i);

            // Split the row based on various line endings
            String[] rowParts = row.split("\\r?\\n|\\n|\\r");

            // Process each part of the row and concatenate into a StringBuilder
            StringBuilder processedRow = new StringBuilder();
            for (String part : rowParts) {
                // Remove line separator characters from the part
                String processedPart = part.replaceAll("\\r?\\n|\\n|\\r", "");

                // Add a comma followed by the first 8 characters of UUID
                processedPart = processedPart + "," + UUID.randomUUID().toString().substring(0, 8);

                // Append it with a line separator
                processedPart = processedPart + System.lineSeparator();

                // Append the processed part to the StringBuilder
                processedRow.append(processedPart);
            }

            // Add the processed row to the result list
            resultList.add(processedRow.toString());
        }

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
