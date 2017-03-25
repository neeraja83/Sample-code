
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.BufferedWriter;
 
import java.io.BufferedReader;
import java.util.Iterator;
import java.lang.Object;
import java.io.IOException;

import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
 
import java.net.URL;
import java.net.URLConnection;

public class URLtest {
 
    @SuppressWarnings("unchecked")
    public static void main(String[] args){
        JSONParser parse = new JSONParser();
 
        try {
 
            //Change the file path here
            Object obj = parse.parse(new FileReader(
                    "C:/glassfish3/jdk7/bin/file1.txt"));
  
 			JSONObject obj1 = new JSONObject();
 			JSONObject obj2 = new JSONObject();
            JSONObject jsonOb = (JSONObject) obj;
 
            JSONArray urlList = (JSONArray) jsonOb.get("Url List");

            System.out.println("\nURL List:");
            Iterator<String> iterator = urlList.iterator();

            do{
		int size;
		String url1=iterator.next();
		System.out.println(url1);
		
              
	        URL url = new URL(url1);
	        URLConnection conn = url.openConnection();
	        size = conn.getContentLength();
	        if (size < 0)
	        System.out.println("No such file exists");
	      else
	        System.out.println("File Size of this url is = "
      +size + " bytes");
		obj2.put("size",size);
	        conn.getInputStream().close();

       
		obj1.put("Url List", url1);
		String url2=obj1.toJSONString();

		 
          File fi =new File("C:/glassfish3/jdk7/bin/file2.txt");
    	  if(!fi.exists()){
    	 	fi.createNewFile();
    	  }
    	  	
		try (FileWriter file = new FileWriter(fi,true)) {
		BufferedWriter bw = new BufferedWriter(file);
    
		file.append(obj1.toJSONString());
		file.append(obj2.toJSONString());
	
			file.flush();
			
} 
          }while (iterator.hasNext());
 
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
