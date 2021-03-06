# My Java Code Snippets
### Jsoup Advance Starter Template

```java
	

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;
import java.security.SecureRandom;
import java.security.cert.X509Certificate;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.util.stream.Collectors;

import javax.net.ssl.HttpsURLConnection;
import javax.net.ssl.SSLContext;
import javax.net.ssl.SSLSocketFactory;
import javax.net.ssl.TrustManager;
import javax.net.ssl.X509TrustManager;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;

import com.mycodes.urlcheck.SSLExceptionSolution;
import com.squareup.okhttp.OkHttpClient;
import com.squareup.okhttp.Request;

public class JsoupStarterTemplate{

	public static void main(String[] args) {

	}


	public static String getURLResponse(String company_url) {
		Document document=null;;
		try {

			SSLExceptionSolution.enableSSLSocket();
			document = Jsoup.connect(company_url).followRedirects(true).userAgent("Mozilla/5.0 (Windows NT 6.1; WOW64; rv:46.0)       Gecko/20100101 Firefox/46.0").ignoreHttpErrors(true)
					.timeout(20000).get();

			if (document == null) {
				OkHttpClient client = new OkHttpClient();
				Request request = new Request.Builder().url(company_url).get()
						.addHeader("cache-control", "no-cache").build();
				String response = client.newCall(request).execute().body().string();
				document = Jsoup.parse(response);
			}

			if (document == null) {
				String line = null;
				String finalline = null;
				URL url = new URL(company_url);
				URLConnection urlConnection = url.openConnection();
				HttpsURLConnection httpsUrlConnection = (HttpsURLConnection) urlConnection;
				urlConnection.setConnectTimeout(20000);
				httpsUrlConnection.setConnectTimeout(20000);
				SSLSocketFactory sslSocketFactory = createSslSocketFactory();
				httpsUrlConnection.setSSLSocketFactory(sslSocketFactory);
				try (InputStream inputStream = httpsUrlConnection.getInputStream()) {
					BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
					while ((line = reader.readLine()) != null) {
						finalline = finalline + line;
					}
				}
				document = Jsoup.parse(finalline);
			}
		} catch (Exception e) {
			return null;
		}
		return document.toString();
	}

	public static SSLSocketFactory createSslSocketFactory() throws Exception {
		TrustManager[] byPassTrustManagers = new TrustManager[] { new X509TrustManager() {
			public X509Certificate[] getAcceptedIssuers() {
				return new X509Certificate[0];
			}

			public void checkClientTrusted(X509Certificate[] chain, String authType) {
			}

			public void checkServerTrusted(X509Certificate[] chain, String authType) {
			}
		} };
		SSLContext sslContext = SSLContext.getInstance("TLS");
		sslContext.init(null, byPassTrustManagers, new SecureRandom());
		return sslContext.getSocketFactory();
	}

}


```
### JSOUP SSLException Solution 

```java

import org.jsoup.Connection;
import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;

import javax.net.ssl.*;
import java.io.IOException;
import java.security.KeyManagementException;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.security.cert.CertificateException;
import java.security.cert.X509Certificate;

public class SSLExceptionSolution {
    
    final static String requestUrl = "https://somewhere.com/target.jsp";

    public static void enableSSLSocket() throws KeyManagementException, NoSuchAlgorithmException {
        HttpsURLConnection.setDefaultHostnameVerifier(new HostnameVerifier() {
            public boolean verify(String hostname, SSLSession session) {
                return true;
            }
        });

        SSLContext context = SSLContext.getInstance("TLS");
        context.init(null, new X509TrustManager[]{new X509TrustManager() {
            public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {
            }

            public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {
            }

            public X509Certificate[] getAcceptedIssuers() {
                return new X509Certificate[0];
            }
        }}, new SecureRandom());
        HttpsURLConnection.setDefaultSSLSocketFactory(context.getSocketFactory());
    }

    public static void main(String[] args) throws IOException, NoSuchAlgorithmException, KeyManagementException {
        String bigno = "S";
        String midno = "11";
        String useno = "111111";
        String chkno = "1";

        enableSSLSocket();

        Connection.Response response = Jsoup.connect(requestUrl)
                .data("bigno", bigno)
                .data("midno", midno)
                .data("useno", useno)
                .data("chkno", chkno)
                .userAgent("Mozilla/5.0 (Windows NT 6.2; WOW64; rv:29.0) Gecko/20100101 Firefox/29.0")
                .method(Connection.Method.POST)
                .execute();

        switch (response.statusCode()) {
            case 200:
                doProcess(response.parse());
                break;
            default:
                
                break;
        }
    }

    public static void doProcess(Document document){
        // do something...
    }
}


```
### Jsoup To Connect Url Step

```java
                Document document=null;
		String response=getURLResponse("https://www.google.com/");
		if(response!=null)
		{
			document=Jsoup.parse(response);
		        ///logic starts here
		}

```

### Extract Data From CSS Query

```java
             Elements name_elments = documnet.select("meta[itemprop='name']");
	     if (name_elments != null && name_elments.size() > 0) {
		for (Element element : name_elments) {
	  	      try {
				title=element.attr("content");	
				//logic starts here
				
				
		           } catch (Exception e) {
			      e.printStackTrace();
		    }	
		}         
	       
```


### Extract Data From Particular Tag

```java
             Elements list_elements = newElement.getElementsByTag("li");
	     if (list_elements != null && list_elements.size() > 0) {
		for (Element element : list_elements){
			try{
			    //logic starts here
			    
			    
			}catch (Exception e) {
				e.printStackTrace();
			}
		}  		
	     }
	       
```

### Extract Data From Particular Tags And With More Conditions

```java
            Elements span_elements = document.select("span");
		if (span_elements != null && span_elements.size() > 0) {
			for (Element element : span_elements) {
				try {
					if (element.hasAttr("class") && element.attr("class").equals("t-20 t-black")) {
						employeecount = element.text().replace("employees", "").replace(",", "").trim();
						  //logic starts here
						  
						
					}
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		}
	       
```


### Extract All Href Link From Any Website

```java          
	   Elements links = document.getElementsByTag("a"); 
	   if (links != null && links.size() > 0) {
		for (Element link: links) {
			try {
				String linkHref = link.attr("href");
				  //logic starts here
				
				
			} catch(Exception e) {
				e.printStackTrace();
			}
		}
	   )
	        
```




