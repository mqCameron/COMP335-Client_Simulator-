
// COMP335 Assignment 1: Stage 1
// 44849516 Cameron Mills and Victor Nikolic 45158290

//input libraries
import java.io.*;
import java.net.*;
import java.util.Arrays;
import java.util.concurrent.TimeUnit;

public class Client {

// initialize the socket and the input and output
    Socket socket = null;
    DataOutputStream output = null;
    DataInputStream input = null;
    String start = "Running";

// Common Variables 
String verify = "OK\n";
String quit = "QUIT";
    
// Part 1 - sending hello
    String Hello = "HELO\n";
    char[] Helloarray = new char[3];
    String HelloS = "null";
    Boolean confirmHello = false;
    int i = 0;

// Part 2 - sending authentication
    String Auth = "AUTH COMP335\n";    
    char[] Autharray = new char[3];
    String AuthS = "null";
    Boolean confirmAuth = false;
    int j = 0;

// Part 3 - scheduling job
    private static final String READY = "REDY";
    private static final String RESC = "RESC Avail %s %s %s";
    private static final String SCHD = "SCHD %s %s %s";
    private static final String LSTJ = "LSTJ %s %s";

// Server Responces  
    private static final String JOBN = "JOBN";
    private static final String NONE = "NONE";
    private static final String DATA = "DATA";
    private static final String ERR = "ERR";
    private static final String DOT_RESPONSE = ".";

// Establish Address and Port numbers
    
    public static void main(String args[]) {
        Client socket = new Client("127.0.0.1", 8096);
    }

// Initialize connection with server
    
    public Client(String address, int port) {
    	try {
    		// connect to server and initialize input and out data stream
    		socket = new Socket(address, port);
    		input = new DataInputStream(socket.getInputStream());
    		output = new DataOutputStream(socket.getOutputStream());
    		
    		//error handling if connection can be established 
    	} catch (UnknownHostException UnKnown) {
    		System.out.println(UnKnown);

    	} catch (IOException IOerror) {
    		System.out.println(IOerror);
    	}

        // send Hello to server
        while (!start.equals("QUIT")) {
            try {
                // Part 1: Send HELO to server
                output.write(Hello.getBytes());

                // while client has not received all data from server
                while (i < Helloarray.length) {
                    // read in a byte and assigns it to an Int
                    int bs = input.read();
                    // converts byte to char
                    char c = (char) bs;
                    // saving char into current array 
                    Helloarray[i] = c;
                    // move index up 1
                    i++;
                } 
                
                // convert array into string 
                String HelloS = new String(Helloarray);
                // print out string
                System.out.println(HelloS);
                // if string = expected output
                if (HelloS.equals(verify)) {
                	confirmHello = true;
                } else {
                    System.out.println("error" + HelloS);
                    input.close();
                    output.close();
                    socket.close();
                }

           // send Auth to server
            output.write(Auth.getBytes());
            
            while (j < Autharray.length) {
                    int bs1 = input.read();
                    char c1 = (char) bs1;
                    Autharray[j] = c1;
                    j++;
                }

                String AuthS = new String(Autharray);
                System.out.println(AuthS);
                if (AuthS.equals(verify)) {
                	confirmAuth = true;
                } else {
                    System.out.println("error" + AuthS);
                    input.close();
                    output.close();
                    socket.close();
                }



                // Part 3: Victor if you want to put your code for the parsing and jobs scheduler below: 
                

  
            } catch (IOException IOerror) {
                System.out.println(IOerror);
            }
        }

        // close the connection
        try {
            input.close();
            output.close();
            socket.close();
        } catch (IOException IOerror) {
            System.out.println(IOerror);
        }
    }
}
