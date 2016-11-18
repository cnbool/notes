title: 使用HttpClient测试网站是否可登录
categories:
  - apache-commons
date: 2015-09-01 16:27:55
tags:
---
```java
/**
 * 测试网站登录
 *
 */
public class TestLoginTask extends MonitorTask {

	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;

	private HttpPost httpPost;

	/**
	 * 
	 * @param clientIp 客户端IP
	 * @param interval 执行间隔，单位毫秒
	 * @param url	      登录提交地址
	 * @param username 用户名
	 * @param pwd	      密码
	 * @throws UnsupportedEncodingException
	 */
	public TestLoginTask(String clientIp, int interval, String url, String username, String pwd) throws UnsupportedEncodingException {
		super(clientIp, interval);
		this.httpPost = new HttpPost(url);
		List<NameValuePair> pairList = new ArrayList<NameValuePair>();
		pairList.add(new BasicNameValuePair("userName", username));
		pairList.add(new BasicNameValuePair("password", pwd));
		httpPost.setEntity(new UrlEncodedFormEntity(pairList));
	}

	@Override
	public TaskResult call() throws Exception {
		CloseableHttpClient httpClient = HttpClients.createDefault();
		CloseableHttpResponse response = null;
		try {
			response = httpClient.execute(httpPost);
			HttpEntity entity = response.getEntity();
			EntityUtils.consume(entity);

			Header[] val = response.getHeaders("Location");
			// 如果跳转到index，登录成功
			if (response.getStatusLine().getStatusCode() == 302 && val.length > 0
					&& val[0].getValue().contains("/index")) {
				// 登录成功
				return new TaskResult(this, State.OK, httpPost.getURI() + "测试登录成功", null);
			} else {
				return new TaskResult(this, State.CRITICAL, httpPost.getURI() + "测试登录失败", null);
			}
		} finally {
			try {
				if (response != null) {
					response.close();
				}
				httpClient.close();
			} catch (IOException e) {
			}
		}
	}

}
```