import java.net.*; 
import java.io.*; 
public class s 
{ 
public static void main (String [] args ) 
throws IOException 
{ 

int A;
         int x[]=new int[512];
         int z[]=new int[512];
		A=Integer.parseInt(args[0]);

System.out.println("Amplitude = " +A);


	    float f;
	    int t=0,T;
	    T=Integer.parseInt(args[1]);
            System.out.println("Time =  "+ T);

            f=(float)1.0/T;
	
		for(t=0;t<T;t++)
		{
			x[t] = (int) (A *Math.sin(2 * 22.0/7 * f * t));
			System.out.print(t+ "     "+x[t]+"\n");
		}
		System.out.println();
		
		int b,size,m;
	
	    b=Integer.parseInt(args[2]);
	
	System.out.println("No of bits for Quantisation code is :"+b);

	m=(int)Math.pow(2,b-1);//no of tyms loop to repeat
	    size=(int)A/(int)Math.pow(2,b-1);                      //no of bits
	       //s[t]=sum over k ak*freq(t-kT)
	       //ak - amplitude k = no of tyms t - no of itn //samples T
	    int p,n,k;
	    for(t=0;t<T;t++)
		{
			    if(x[t]>=0)
			    {
			    p=0;n=size;
			    for(k=0;k<m;k++)
			    {
			     if(x[t]>=p&&x[t]<=n)
			     {
				     z[t]=k+m;
					break;
			     }
				p=n;
				n=n+size;
			    }

			    }
			    else
			    {
				p=-1;n=-size;
			    for(k=0;k<m;k++)
			    {
			     if(x[t]<=p&&x[t]>=n)
			     {
				     z[t]=m-k-1;
					break;
			     }
					p=n;
				    n=n-size;
			    }

			    }
		System.out.print(t + "    " + z[t]+"\n");
		}

                System.out.println();

		int i,rem,j=0,sum,hh=0;
		int sum1[]= new int[100];
                 for(j=0;j<T;j++)
		{
			i=1;    sum=0;
			do
			{
				rem=z[j]%2;
				sum=sum + (i*rem);
				z[j]=z[j]/2;
				i=i*10;
                        }while(z[j]>0);

			System.out.print(sum+" ");
			 sum1[j]=sum;
		        try
			{
			PrintStream outy = new PrintStream(new FileOutputStream("ans.txt"));
		        for(int ii = 0; ii <=j; ii++)
                              { outy.println(sum1[ii]);   }

			}
			catch(IOException e1)
			{
				System.out.println("Exception in file");
			}
			

		}
		ServerSocket serverSocket = new ServerSocket(1515); 
		Socket socket = serverSocket.accept(); 
		//System.out.println("Accepted connection : " + socket); 
		File transferFile = new File ("ans.txt"); 
		byte [] bytearray = new byte [(int)transferFile.length()]; 
		FileInputStream fin = new FileInputStream(transferFile); 
		BufferedInputStream bin = new BufferedInputStream(fin); 
		bin.read(bytearray,0,bytearray.length); 
		OutputStream os = socket.getOutputStream(); 
		
		os.write(bytearray,0,bytearray.length); 
		os.flush(); 
		socket.close(); 
		
	} 
};


