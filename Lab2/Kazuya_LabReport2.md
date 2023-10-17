# Lab Report 2                          Kazuya Miyata

# String Server Implementation:  
```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    //Stores the Strings and the total number of Strings
    String output = "";
    int num = 0;

    public String handleRequest(URI url) {
        //Checks if path is valid
        if (url.getPath().equals("/add-message")) {
            //Creates String array to split parts of the query
             String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    //If the format is correct, increment number of Strings and concatnate to output
                    num++;
                    output += "\n" + num + ". " + parameters[1];
                    //Returns the output 
                    return output;
                } else {
                    //If the format of the query is invalid
                    return "Invalid Format!";
                }


        } else { //Returns if path is invalid
            return "404 Not Found!";
        }
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

**Examples of the code Working**  
![Image](images/lab2_1.png)  
![Image](images/lab2_2.png) 
