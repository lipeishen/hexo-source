title: Java Socket 编程
date: 2014-04-28 16:38:22
tags: Socket
language: ch
categories: Java
---
<!--more-->
## 单用户请求的socket 

### 服务器端代码：
   //Server_Socket.java
```   
import java.io.*;
import java.net.*;


public class Server_Socket {


public static final int PORT=8083;
 

	public static void main(String[] args) throws IOException{
      ServerSocket s=new ServerSocket(PORT);
      System.out.println("Started:"+ s);
      try {
    	  Socket socket=s.accept();
		  try {
			  
			  System.out.println("Connection accpted:"+socket);
			  BufferedReader in=new BufferedReader(new InputStreamReader(socket.getInputStream()));
			  PrintWriter out=new PrintWriter(new BufferedWriter(new OutputStreamWriter(
					  
					  
					  socket.getOutputStream())),true);
			  while(true){
				  
				  String str=in.readLine();
				  if(str.equals("END"))
					  break;
				  System.out.println("Enchoing"+str);
				  out.println(str + str +"hahahaha");
				  
			  }
			  
			
		} finally{
			System.out.println("closing......");
			socket.close();
		}
    	  
    	  
    	  
    	  
	} finally {
		s.close();
		
	}


	}


	

}

```

###客户端代码

 //Client_Socket.java
 
 ```
 import java.io.*;
import java.net.*;
public class Client_Socket {

	
	public static void main(String[] args) throws IOException {
		 InetAddress addr=InetAddress.getByName(null);
		 System.out.println("addr="+addr);
		 Socket socket=new Socket(addr, Server_Socket.PORT);
		 try {
			 
			 System.out.println("socket="+socket);
			 
			 BufferedReader in=new BufferedReader(new InputStreamReader(socket.getInputStream()));
			  PrintWriter out=new PrintWriter(new BufferedWriter(new OutputStreamWriter(
					  
					  
					  socket.getOutputStream())),true);
			  for (int i = 0; i < 10; i++) {
				  out.println("www"+i);
				  String str=in.readLine();
				  System.out.println(str);
				
			}
			out.println("END");
		} finally {
			
			System.out.println("closing.......");
			socket.close();
			
		}
		
	}

}
```

##多用户请求的Socket

###服务器端：
//Mult.java

```
import java.io.*;
import java.net.*;

public class Mult extends Thread{
	private Socket socket;
	private BufferedReader in;
	private PrintWriter out;
	
  public Mult(Socket s) throws IOException {
	  socket=s;
	  in=new BufferedReader(new InputStreamReader(socket.getInputStream()));
	  out=new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())),true);
	  
	  start();
	
}
  
  public void run(){
	  
	  try {
		  while (true) {
			String string=in.readLine();
			if (string.equals("END"))  
				 break;
			System.out.println("Echoing"+string);
			out.println(string);
			
		}
		  System.out.println("closing....");
		
	} catch (Exception e) {
		
	}finally{
		
		try {
			socket.close();
		} catch (IOException e2) {
			
		
		
	}
	}
	  
  }
	
}
```

//ServerSocketMult.java

```
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;


public class ServerSocketMult {

  static final int PORT=8085;	
	public static void main(String[] args) throws IOException{
		 ServerSocket s=new ServerSocket(PORT);
		 System.out.println("Server Started");
		 try {
			 while (true) {
				 Socket socket=s.accept();
				 try {
					 new Mult(socket);
					
				} catch (IOException e) {
					socket.close();
				}
				
				
			}
			
		} finally {
			s.close();
		}
		

	}

}
```


###客户端代码：

//ClientSocketMultThread.java

```
import java.io.*;
import java.net.*;
public class ClientSocketMultThread extends Thread {
	private Socket socket;
	private BufferedReader in;
	private PrintWriter out;
	private static int counter=0;
	private int id=counter++;
	private static int threadCount=0;
	public static int  threadCount() {
         return threadCount;		
	}
	public ClientSocketMultThread(InetAddress addr){
		
		System.out.println("Making client"+id);
		threadCount++;
		try {
			socket=new Socket(addr,ServerSocketMult.PORT);
		} catch (IOException e) {
			
		}
		try {
			 in=new BufferedReader(new InputStreamReader(socket.getInputStream()));
			  out=new PrintWriter(new BufferedWriter(new OutputStreamWriter(socket.getOutputStream())),true);
			  
			  start();
		} catch (IOException e) {
			try {
				socket.close();
			} catch (IOException e2) {
				
			
			
		}
		}
		
		
		
	}
	
	public void run () {
		try{
			for (int i = 0; i < 25; i++) {
				out.println("Client"+id+":"+i);
				String str=in.readLine();
				System.out.println(str);
			}
			out.println("END");
		}catch (IOException e) {
			
		}finally{
			
			try {
				socket.close();
			} catch (IOException e) {
				
			
			
		}
			threadCount--;
		}
		
	}

}
```

//ClientSocketMult.java

```
import java.io.IOException;
import java.net.InetAddress;
public class ClientSocketMult {
static final int MAX_THREADS=40;
	
	public static void main(String[] args) throws IOException,InterruptedException{
		InetAddress addr=InetAddress.getByName(null);
		while (true) {
			if(ClientSocketMultThread.threadCount()<MAX_THREADS)
				 new ClientSocketMultThread(addr);
			Thread.sleep(100);
			
		}

	}

}
```

##数据报通信（UDP）

//DataGram.java

```
import java.net.*;
public class DataGram {
	public static DatagramPacket toDatagramPacket(String s,InetAddress destIA,int destPort)
	{
		byte[] buf=new byte[s.length()+1];
		s.getBytes(0,s.length(),buf,0);
		return new DatagramPacket(buf, buf.length,destIA,destPort);
		
		
		
	}


	public static String toString(DatagramPacket p) {
		// TODO Auto-generated method stub
		return new String(p.getData(),0,p.getLength());
	}
	

	
}
```
###服务端程序

//UDP_socket_server.java

```
import java.io.*;
import java.net.*;
import java.util.*;

public class UDP_socket_server {
	static final int INPORT=1712;
	private byte[] buf=new byte[1000];
	private DatagramPacket dp=new DatagramPacket(buf, buf.length);
	private DatagramSocket socket;
	
	public UDP_socket_server() {
		try {
			socket=new DatagramSocket(INPORT);
			System.out.println("Server Started");
			while(true){
				
				socket.receive(dp);
				String rcvd=DataGram.toString(dp)+", from Address:"+dp.getAddress()
				+"port:"+dp.getPort();
				System.out.println(rcvd);
				String echoString="Echoed:"+rcvd;
				DatagramPacket echo=DataGram.toDatagramPacket(echoString, dp.getAddress(), dp.getPort());
				socket.send(echo);
				
			}
			
			
		} catch (SocketException e) {
			System.err.println("can't open socket!");
			System.exit(1);
			
		}catch (IOException e) {
			System.err.println("Communication error");
			e.printStackTrace();
			
		}
		
	}



	public static void main(String[] args) {
		new UDP_socket_server();
	

	}

}
```
###客户端程序

//UDP_socket_client.java

```
import java.io.*;
import java.net.*;




public class UDP_socket_client extends Thread{
	private DatagramSocket s;
	private InetAddress hostAddress;
	private byte[] buf= new byte[1000];
	private DatagramPacket dp =new DatagramPacket(buf, buf.length);
	private int id;
	public UDP_socket_client(int identifier){
		
		id=identifier;
		try {
			s=new DatagramSocket();
			hostAddress=InetAddress.getByName("localhost");
			
		} catch (UnknownHostException e) {
			System.err.println("can't find host!");
			System.exit(1);
			
		}catch (SocketException e) {
			System.err.println("can't open socket!");
			e.printStackTrace();
			System.exit(1);
		}
		System.out.println("UDP_socket_client starting");
		
		
		
	}
	public void run(){
		
		try {
			for (int i = 0; i < 25; i++) {
				String  outMessage ="Client #"+id+",message #"+i;s.send(DataGram.toDatagramPacket(outMessage, hostAddress, UDP_socket_server.INPORT));
				s.receive(dp);
				String rcvd="Client #"+id+",rcvd from"+dp.getAddress()+","+dp.getPort()+
				":"+DataGram.toString(dp);
				System.out.println(rcvd);
				
				 
			}
		} catch (IOException e) {
			e.printStackTrace();
			System.exit(1);
			
		}
		
		
	}

	
	public static void main(String[] args) {
		for (int i = 0; i < 10; i++) {
			new UDP_socket_client(i).start();
		}
		

	}

}
```


