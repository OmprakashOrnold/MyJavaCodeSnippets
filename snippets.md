# My Java Code Snippets
# Jsoup Snippets

```
	public static String getURLResponse(String company_url) {
		Document document=null;;
		try {

			SSLExceptionSolution.enableSSLSocket();
			document = Jsoup.connect(company_url).followRedirects(true).userAgent("Mozilla/5.0 (Windows NT 6.1; WOW64; rv:46.0) Gecko/20100101 Firefox/46.0").ignoreHttpErrors(true)
					.timeout(20000).get();

			if (document == null) {
				OkHttpClient client = new OkHttpClient();
				Request request = new Request.Builder().url(company_url).get()
						.addHeader("cache-control", "no-cache").build();
				String jsonResponse = client.newCall(request).execute().body().string();
				document = Jsoup.parse(jsonResponse);
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

	private static SSLSocketFactory createSslSocketFactory() throws Exception {
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


```
