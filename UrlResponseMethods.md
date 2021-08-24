# MyJavaCodeSnippets
# Url Response Using URL Connection

```
public static String  getResponseURLConnection(String url) {
		StringBuilder result = new StringBuilder();
		String line;
		try{
			URLConnection urlConnection = new URL(url).openConnection();
			urlConnection.addRequestProperty("User-Agent", "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)");
			urlConnection.setReadTimeout(5000);
			urlConnection.setConnectTimeout(5000);
			urlConnection.connect();
			try (InputStream is = new URL(url).openStream();
					BufferedReader br = new BufferedReader(new InputStreamReader(is))) {
				while ((line = br.readLine()) != null) {
					result.append(line);
				}
			}
		} catch (Exception e) {
			return e.getMessage();
		}
		return result.toString();
	}
  
```

