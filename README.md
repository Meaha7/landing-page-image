import java.util.List;

public class MessageProcessor {
    public static void processMessages(List<String> outputMessages) {
        for (int i = 0; i < outputMessages.size(); ) {
            String msg = outputMessages.get(i);

            if (msg.contains("I564") || msg.contains("I566")) {
                int startIndex = msg.indexOf("{3:108:");
                int endIndex = msg.indexOf("}}", startIndex) + 2; // Include the closing braces

                // Remove substring between "{3:108:" and "}}"
                outputMessages.set(i, msg.substring(0, startIndex) + msg.substring(endIndex));
                i++; // Move to the next index after processing
            } else {
                // Remove messages not containing "I564" or "I566"
                outputMessages.remove(i);
                // No need to increment 'i' here, as the next element will take the current index
            }
        }
    }

    public static void main(String[] args) {
        // Assuming Outputmsg is a List<String> containing your messages
        List<String> outputMessages = ...; // Initialize with your actual data

        processMessages(outputMessages);
        // Now, outputMessages contains the modified messages
    }
}
