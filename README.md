# Java-web-scraping
<h2>Quick guide with code example how to use Java for web scraping</h2>

![java web scraping header](https://github.com/luminati-io/java-web-scraping/blob/main/Web%20scraping%20with%20Java%20-%20Ultimate%20guide.png "java scraping guide banner")

While some people prefer using Python, another popular option is **utilizing Java for web scraping**.  Here is a step-by-step guide of how to easily accomplish this.

Before you begin, ensure that you have the following set up on your computer so that the environment is optimal for web scraping:

[Java11](https://www.java.com/en/download/help/download_options.html) -There are more advanced versions but this remains by far the most popular among developers.

[Maven](https://maven.apache.org/) – Is a building automation tool for dependency management

[IntelliJ IDEA](https://www.jetbrains.com/idea/download/#section=windows) – IntelliJ IDEA is an integrated development environment for developing computer software written in Java.

[HtmlUnit](https://htmlunit.sourceforge.io/) – This is a browser activity simulator (e.g. form submission simulation).

You can check installations with these commands:

* ```java -version```
* ```mvn -v```

<h3>Step One: Inspect your target page</h3>
Head to the target site that you would like to collect data from, right click anywhere and hit ‘inspect element’ in order to access the ‘Developer Console’, which will grant you access to the web page's HTML.
<h3>Step Two: Begin scraping the HTML</h3>
Open IntelliJ IDEA and create a Maven project:

![intellij IDEA Maven project](https://github.com/luminati-io/java-web-scraping/blob/main/Java2%20intellij.png "intellij maven project")

Maven projects have a pom.xml file. Navigate to the pom.xml file, and first set up the JDK version for your project:

```
<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>11</maven.compiler.source>
		<maven.compiler.target>11</maven.compiler.target>
	</properties>
  ```
  And then add the **HtmlUnit** dependency to the **pom.xml** file as follows:
  
  ```
  <dependencies>
		<dependency>
			<groupId>net.sourceforge.htmlunit</groupId>
			<artifactId>htmlunit</artifactId>
			<version>2.63.0</version>
		</dependency>
	</dependencies>
  ```
  Now everything is set-up to begin writing the first Java class. Start by creating a new Java source file like so:
  
  ![open new java source](https://github.com/luminati-io/java-web-scraping/blob/main/Java3%20intellij.png "new java class")
  
  We need to create a main method for our application to start. Create the main method like this:
  
  ```
  public static void main(String[] args) throws IOException {
   }
  ``` 
  The app will start with this method. It is the application entrypoint. You can now send an **HTTP request** using **HtmlUnit** imports as follows:
  
  ```   
  import com.gargoylesoftware.htmlunit.*;
  import com.gargoylesoftware.htmlunit.html.*;
  import java.io.IOException;
  import java.util.List;
  ```  
  Now create a ```WebClient``` by setting the options as follows:
  
  ``` 
  private static WebClient createWebClient() {
		WebClient webClient = new WebClient(BrowserVersion.CHROME);
		webClient.getOptions().setThrowExceptionOnScriptError(false);
		webClient.getOptions().setCssEnabled(false);
	  webClient.getOptions().setJavaScriptEnabled(false);
		return webClient;
	}
  ``` 
  
<h3>Step Three: Extract/parse the data from the HTML</h3>
Now let’s extract the target price data that we are interested in.
We will use the following **HtmlUnit** built-in commands in order to accomplish this. Here is what that would look like for data points pertaining to **product price**:

```
WebClient webClient = createWebClient();
	    
		try {
			String link = "https://www.ebay.com/itm/332852436920?epid=108867251&hash=item4d7f8d1fb8:g:cvYAAOSwOIlb0NGY";
			HtmlPage page = webClient.getPage(link);
			
			System.out.println(page.getTitleText());
			
			String xpath = "//*[@id=\"mm-saleDscPrc\"]";			
			HtmlSpan priceDiv = (HtmlSpan) page.getByXPath(xpath).get(0);			
			System.out.println(priceDiv.asNormalizedText());
			
			CsvWriter.writeCsvFile(link, priceDiv.asNormalizedText());
			
		} catch (FailingHttpStatusCodeException | IOException e) {
			e.printStackTrace();
		} finally {
			webClient.close();
		}	
  ```
To get the **XPath** of the desired element, go ahead and use the Developer Console. On the Developer Console, right-click the selected section and click “Copy XPath”. This command will copy the selected section as an XPath expression:

![xpath of element](https://github.com/luminati-io/java-web-scraping/blob/main/Java4.png "get xpath")

The web pages contain links, text, graphics, and tables. If you select an XPath of a table, you can export it to CSV and make further calculations, and analysis with programs such as Microsoft Excel. In the next step, we will examine exporting a table as a CSV file.

<h3>Step Four: Exporting the data</h3>
Now that the data has been parsed, we can export it into CSV format for further analysis. This format may be preferred by certain professionals over others, as it can then be easily opened/viewed in Microsoft Excel. Here are the command lines to use in order to accomplish this:

```
public static void writeCsvFile(String link, String price) throws IOException {
		
		FileWriter recipesFile = new FileWriter("export.csv", true);

		recipesFile.write("link, price\n");

		recipesFile.write(link + ", " + price);

		recipesFile.close();
	}
  ```

----

Although Java can help professionals in various fields extract the data they need, the process of web scraping can be quite time-consuming. To fully automate your data collection operations you can utilize a tool like the Bright [Data Collector](https://brightdata.grsm.io/vitariz-dca). All you need to do is choose their target site, and output dataset, and then select your desired schedule, file format, and delivery method.
