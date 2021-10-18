# My Java Code Snippets

### Read JSON Url Using BufferReader

```java
   
public static JSONObject readJsonFromUrl(String url) throws IOException, JSONException {
		InputStream is = new URL(url).openStream();
		try {
			BufferedReader rd = new BufferedReader(new InputStreamReader(is, Charset.forName("UTF-8")));
			String jsonText = readAll(rd);
			JSONObject json = new JSONObject(jsonText);
			return json;
		} finally {
			is.close();
		}
	}

public static String readAll(Reader rd) throws IOException {
		StringBuilder sb = new StringBuilder();
		int cp;
		while ((cp = rd.read()) != -1) {
			sb.append((char) cp);
		}
		return sb.toString();
	}

```
### Read JSON Url Using OkHttpClient

```java
public static String getUrlResponse(String search_url) {
  
	String jsonResponse = null;
	try {

		OkHttpClient client = new OkHttpClient();
		Request request = new Request.Builder().url(search_url).get().addHeader("cache-control", "no-cache").build();
		jsonResponse = client.newCall(request).execute().body().string();

	} catch(Exception e) {
		e.printStackTrace();
	}
	return jsonResponse;
}
```

### Validate It Is JSONObject Or not

```java 
public static boolean isValid(JSONObject jsonObject, String propertyName) {
		boolean valid = false;
		try {
			if (propertyName != null) {
				if (jsonObject.has(propertyName) && !jsonObject.get(propertyName).toString().equalsIgnoreCase("null")
						&& !jsonObject.get(propertyName).toString().equalsIgnoreCase(""))
					valid = true;
			}
		} catch (Exception ex) {
			System.out.println(ex.toString());
			valid = false;
		}
		return valid;
	}


```

### Extact Data From JSONObject

```java
 if (jsonResponse != null) {
	JSONObject jsonObject = new JSONObject(jsonResponse);
	JSONObject propsJsonObject = (JSONObject) jsonObject.get("props");
	if (propsJsonObject != null && propsJsonObject.has("pageProps")) {
		//logic goes on here
	}
}
```

### Extact Data From JSONArray

```java
if (initialStateJsonObject != null && initialStateJsonObject.has("teamName")) {
	JSONArray cgArray = initialStateJsonObject.getJSONArray("cg");
	int arraySize = cgArray.length();
	if (arraySize > 0) {
		for (int arrayIndex = 0; arrayIndex < arraySize; arrayIndex++) {
			try {
			JSONObject categoryFeaturesObject = categoryFeaturesArray.getJSONObject(arrayIndex1);
			
			if (categoryFeaturesObject != null&& categoryFeaturesObject.has("name")) {
												if (isValid(categoryFeaturesObject, "name")) 
												{																
													String namess= categoryFeaturesObject.get("name").toString();
													list.add(namess);															
												}
											}

                           } catch(Exception e) {
				e.printStackTrace();
			}
		}
	}
}

```

### Checking IsValid Method and Exract Particular Field Name
```java
if (isValid(companyBasicInfoObject, "shortName")) {
		shortName= companyBasicInfoObject.get("shortName").toString().trim();										
		}

```

### Example To Read a JSON Url

```java
public static String  getRedirectUrl(String domain) {

		String isDown =null;
		String returnedUrl="";
		try {
			JSONObject jsonObject = readJsonFromUrl("https://api-prod.downfor.cloud/httpcheck/"+domain);
			if (jsonObject != null) {
				if (jsonObject != null && jsonObject.has("isDown")) {
					if (isValid(jsonObject, "isDown")) {
						isDown =jsonObject.get("isDown").toString();										
					}
				}	
				if(isDown.equals("false")){
					if (jsonObject != null && jsonObject.has("returnedUrl")) {
						if (isValid(jsonObject, "returnedUrl")) {
							returnedUrl =jsonObject.get("returnedUrl").toString();
						}
					}
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return returnedUrl; 
	}
```

### Example To Read a JSON many files

```java

import java.io.File;
import java.nio.file.Files;
import java.nio.file.Paths;

import org.json.JSONArray;
import org.json.JSONObject;

public class ExtractCategories {

	public static void main(String[] args) {


		String jsonData=null;

		File folder = new File("C:\\Users\\OmPrakashPeddamadtha\\Downloads\\g2_category_pages");
		File[] listOfFiles = folder.listFiles();
		for (File file : listOfFiles) {
			if (file.isFile()) {
				try {
					String fullFileName = "C:\\Users\\OmPrakashPeddamadtha\\Downloads\\g2_category_pages\\"+file.getName();
					jsonData=readFileAsString(fullFileName);

					if (jsonData != null) {
						
						
					}


				}catch(Exception e)
				{
					e.printStackTrace();
				}
			}
		}
	}

	public static boolean isValid(JSONObject jsonObject, String propertyName) {
		boolean valid = false;
		try {
			if (propertyName != null) {
				if (jsonObject.has(propertyName) && !jsonObject.get(propertyName).toString().equalsIgnoreCase("null")
						&& !jsonObject.get(propertyName).toString().equalsIgnoreCase(""))
					valid = true;
			}
		} catch (Exception ex) {
			System.out.println(ex.toString());
			valid = false;
		}
		return valid;
	}




	public static String readFileAsString(String fileName)throws Exception 
	{ 
		String data = ""; 
		data = new String(Files.readAllBytes(Paths.get(fileName))); 
		return data; 
	}
}

```
