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

In this implementation:
* The `main` and `handleRequest` methods are called
* In the `main` method:
    * The relevant argument is the `args` parameter of type `String[]`. I am expected to pass in an appropriate port number as a command-line argument
    * The relevant field is `port` whose value is dependent on the command-line argument when I run the program
         * The value of `port` is changed when a value is extracted from the `args` array and parsed into an `Integer`, then assigned to `port`
* In the `handleRequest` method:
    * The relevant argument is the `url` objects of type `URI`, which passes in the URL of the HTTP request
    * The relevant field is `chatMessages` of type `ArrayList<String>`
         * The value of `chatMessages` updates as new chat messages are added with each `/add-message` request. The message combines the `user` and `s` parameters from the URL, and the formatted message is then added to the `chatMessages` list
    * From this screenshot, here are the *incoming requests*
         * `/add-message?s=Hey Patrick&user=Spongebob`
         * `/add-message?s=Hey Spongebob&user=Patrick`
      


<img width="482" alt="Lab Report 2 SC 2" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/c8627035-9a2b-44a3-bae6-a472d734089c">

In this implementation:
* The `main` and `handleRequest` methods are called
* In the `main` method, as previously:
    * The relevant argument is the `args` parameter of type `String[]`. I am expected to pass in an appropriate port number as a command-line argument
    * The relevant field is `port` whose value is dependent on the command-line argument when I run the program
         * The value of `port` is changed when a value is extracted from the `args` array and parsed into an `Integer`, then assigned to `port`
* The `main` method again only handles the usage of a correct port, and since I didn't change the port, it won't be directly involved in handling the incoming HTTP requests
* In the `handleRequest` method, as previously:
    * The relevant argument is the `url` objects of type `URI`, which passes in the URL of the HTTP request
    * The relevant field is `chatMessages` of type `ArrayList<String>`
         * The value of `chatMessages` updates as new chat messages are added with each `/add-message` request. The message combines the `user` and `s` parameters from the URL, and the formatted message is then added to the `chatMessages` list
    * From this screenshot, here are the *incoming requests*
         * `/add-message?s=Hey Patrick&user=Spongebob`
         * `/add-message?s=Hey Spongebob&user=Patrick`
         * `/add-message?s=Hey Spongebob&user=Patrick`
         * `/add-message?s=Let's go jellyfishing&user=Spongebob`
    * I refreshed the second `/add-message` request page, which added the same message twice

 In both of my screenshots, the handleRequest method will identify the `user` and `message` passed in through typing in the URL, and will determine if the format of the HTTP request is valid or not. If it is not, a series of error messages will be returned depending on whatever mistake was made:

<img width="538" alt="Lab Report 2 SC 3" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/b888fa63-74ac-471f-b119-829c85a9af41">

Here for example, I didn't specify any query parameters. 

### Part 2

Screenshot showcasing absolute file paths for public and private SSH keys:

<img width="422" alt="Lab Report 2 SC 4" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/f4961f3f-5c59-48c6-a19f-639afb7b33b8">

Screenshot showcasing terminal interaction logging into ieng6 *without* using a password:

<img width="689" alt="Lab Report 2 SC 5" src="https://github.com/ejchas/cse15l-lab-reports/assets/156235662/e63ca4f9-9350-4963-8073-de1b6d96f677">

### Part 3

There were many things that I learned about in the labs of Week 2 and Week 3 that I hadn't known about previously. For one, this was my initial experience using the `ssh` command to connect to a remote server. The entire concept was foreign to me, and I had much difficulty trying to understand the significance of what I was doing. I also learned about the `mkdir` and `scp` commands, which create directories and copy files between the host server and remote server, respectively. These are the fundamentals to understanding how to manage files and securely communicate between machines. Overall, these labs proved to be very challenging for me to get through, but I hope to gather more knowledge as the quarter progresses.

