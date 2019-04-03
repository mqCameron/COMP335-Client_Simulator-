
// COMP335 Assignment 1: Stage 1
// 44849516 Cameron Mills and Victor 

//input libraries
import java.io.*;
import java.net.*;
import java.util.Arrays;

public class Client {

// initialize the socket and the input and output
	Socket socket = null;
	DataOutputStream output = null;
	DataInputStream input = null;
	String start = "Running";

// Part 1 - sending hello
	String Hello = "HELO\n";
	char[] array = new char[3];
	String verify = "OK\n";
	String HelloS = "null";
	Boolean confirm1 = false;
	int i = 0;

// Part 2 - sending authentication
	String Auth = "AUTH XXXX\n";	
	char[] array1 = new char[3];
	String AuthS = "null";
	String verify1 = "OK\n";
	Boolean confirm2 = false;
	int j = 0;


// Part 3 - scheduling job
	String Ready = "REDY\n";

// Establish Address and Port numbers
	
	public static void main(String args[]) {
		Client socket = new Client("127.0.0.1", 8096);
	}

// Initialize connection with server
	
	public Client(String address, int port) {
		try {
			// connect to server
			socket = new Socket(address, port);
			System.out.println("Connected");

			// Initialize input and out data stream
			input = new DataInputStream(socket.getInputStream());
			output = new DataOutputStream(socket.getOutputStream());

		} catch (UnknownHostException u) {
			System.out.println(u);

		} catch (IOException i) {
			System.out.println(i);
		}

		// send messages to server
		while (!start.equals("QUIT")) {
			try {
				// Part 1: Send HELO to server
				output.write(Hello.getBytes());

				// while client has not received all data from server
				while (i < array.length) {
					// read in a byte and assigns it to an Int
					int bs = input.read();
					// converts byte to char
					char c = (char) bs;
					// saving char into current array 
					array[i] = c;
					// move index up 1
					i++;
				}
				
				// convert array into string 
				String HelloS = new String(array);
				// print out string
				System.out.println(HelloS);
				// if string = expected output
				if (HelloS.equals(verify)) {
					confirm1 = true;
				} else {
					System.out.println("error" + HelloS);
					input.close();
					output.close();
					socket.close();
				}

				// Part 2 Auth
				if (confirm1 == true) {
					output.write(Auth.getBytes());
				}

				while (j < array1.length) {
					int bs1 = input.read();
					char c1 = (char) bs1;
					array1[j] = c1;
					j++;
				}

				String AuthS = new String(array1);
				System.out.println(AuthS);
				if (AuthS.equals(verify1)) {
					confirm2 = true;
				} else {
					System.out.println("error" + AuthS);
					input.close();
					output.close();
					socket.close();
				}

				// Part 3 Ready for job
				// Send Ready
					output.write(Ready.getBytes());

					// Receive JOBN and info or receive none
					byte[] b = new byte[input.read()];
					input.read(b);
					String str = new String(b);
					System.out.println(str);

					start = "QUIT";

					// Send Schedule
					// output.write(?????.getBytes());
				

			} catch (IOException i) {
				System.out.println(i);
			}
		}
		

		// close the connection
		try {
			input.close();
			output.close();
			socket.close();
		} catch (IOException i) {
			System.out.println(i);
		}
	}
}
