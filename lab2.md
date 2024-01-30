### Part 1

I created a server called `ChatServer` which allows users to send messages to each other using URL's.

Below is the code that I wrote for `ChatServer.java`: 

```
import java.io.IOException;
import java.net.URI;
import java.util.List;
import java.util.ArrayList;

class ChatHandler implements URLHandler {
    private List<String> chatMessages = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Chat Messages: \n%s", String.join("\n", chatMessages));
        } else if (url.getPath().equals("/add-message")) {
            String query = url.getQuery();
            if (query != null) {
                String[] parameters = query.split("&");
                String user = null;
                String message = null;

                for (String parameter: parameters) {
                    String[] keyValue = parameter.split("=");
                    if (keyValue.length == 2) {
                        if (keyValue[0].equals("s")) {
                            message = keyValue[1];
                        } else if (keyValue[0].equals("user")) {
                            user = keyValue[1];
                        }
                    }
                }

                if (user!= null && message != null) {
                    String newMessage = String.format("%s: %s", user, message);
                    chatMessages.add(newMessage);
                    return String.format("Chat Messages:\n%s", String.join("\n"), chatMessages);
                } else {
                    return "400 Bad Request - Missin required parameters (s and user)";
                }
            } else {
                return "404 Bad Request - Missing query parameters";
            }
        } else {
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new ChatHandler());
    }
}
```

