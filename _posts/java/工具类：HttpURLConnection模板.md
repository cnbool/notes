title: 工具类：HttpURLConnection模板
id: 998
categories:
  - Java
date: 2013-08-13 02:00:14
tags:
---

<!--more-->
**源码:**
```java
package com.cnbool.util;

import java.io.InputStream;
import java.net.HttpURLConnection;
import java.net.URL;

/**
 * 
 * @version 1.0
 *
 */
public class HttpURLConnTemplete {
	public static final String GET = "GET";
	public static final String POST = "POST";
	private String mothod = GET;
	private InputStream in = null;
	private HttpURLConnection httpConn = null;

	public interface DownloadHander {
		public void handle(InputStream in);
		public void handleErr(InputStream in, int respCode);
	}

	public void download(URL url, DownloadHander downloadHander) throws Exception {
		try {
			httpConn = (HttpURLConnection) url.openConnection();
			HttpURLConnection.setFollowRedirects(true);
			httpConn.setRequestMethod(mothod);

			if (httpConn.getResponseCode() == 200) {
				in = httpConn.getInputStream();
				if (downloadHander != null) {
					downloadHander.handle(in);
				}
			} else {
				in = httpConn.getErrorStream();
				if (downloadHander != null) {
					downloadHander.handleErr(in, httpConn.getResponseCode());
				}
			}
		} catch (Exception e) {
			throw e;
		} finally {
			if (in != null) {
				in.close();
			}
			if (httpConn != null) {
				httpConn.disconnect();
			}
		}
	}

	public void setRequestMethod(String method) {
		this.mothod = method;
	}
}
```
