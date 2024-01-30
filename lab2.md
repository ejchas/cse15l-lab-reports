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

**Screenshots of my output:**

<img width="521" alt="Lab Report 2 SC 1" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/94ca27d8-9f99-4b6c-aa18-5fac28128112">

In this image:
* The `main` and `handleRequest` methods are called
* In the `main` method:
    * The relevant argument is the `args` parameter of type `String[]`. I am expected to pass in an appropriate port number as a command-line argument
    * The relevant field is `port` whose value is dependent on the command-line argument when I run the program
         * The value of `port` is changed when a value is extracted from the `args` array and parsed into an `Integer`, then assigned to `port`
* In the `handleRequest` method:
    * 





