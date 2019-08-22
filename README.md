# 封装了HTTPclient连接池,简化创建方式，提高效率，不需要额外new一堆类，直接使用方便快捷
注意：使用完成一定要手动回收一下，apiClientc.releaseConnection(httpGet/httpPost);

SpringBoot 依赖
```xml
                <dependency>
			<groupId>com.httpyu</groupId>
			<artifactId>spring-boot-start-clientc</artifactId>
			<version>0.0.1-SNAPSHOT</version>
		</dependency>
```
使用方式在配置文件中加入
```xml
http.clientc.maxtotal=5 //最大连接
http.clientc.maxperRoute=5 //并发数
```
举例
```java
 @Autowired
   private ApiClientc apiClientc;
   @Autowired
	MaxConfig maxConfig;
   @Test
	public void contextLoads() {
	    System.out.println(maxConfig.getMaxtotal());
	    System.out.println(maxConfig.getMaxperRoute());
		CloseableHttpClient simpleClient = apiClientc.getSimpleClient();
		Map map = new HashMap<>();
		map.put("symbol","ethusdt");
		Map header = new HashMap();
		header.put("user-agent","Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36");
		HttpGet httpGet = apiClientc.buildProxyGet("https://api.huobi.pro/market/detail/merged", map, header,"35.235.75.244",3128);
		try {
			CloseableHttpResponse execute = simpleClient.execute(httpGet);
			String result = apiClientc.getResult(execute);
			System.out.println(result);
			apiClientc.releaseConnection(httpGet);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
```
普通jar使用方式,导入到工程内
```java
 MaxConfig maxConfig = new MaxConfig();
        maxConfig.setMaxtotal(10);
        maxConfig.setMaxperRoute(10);
        ApiClientc apiClientc = new ApiClientc(maxConfig);
        CloseableHttpClient simpleClient = apiClientc.getSimpleClient();
        Map map = new HashMap<>();
        map.put("symbol","ethusdt");
        Map header = new HashMap();
        header.put("user-agent","Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36");
        HttpGet httpGet = apiClientc.buildProxyGet("https://api.huobi.pro/market/detail/merged", map, header,"35.235.75.244",3128);
        try {
            CloseableHttpResponse execute = simpleClient.execute(httpGet);
            String result = apiClientc.getResult(execute);
            System.out.println(result);
            apiClientc.releaseConnection(httpGet);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
