title: 工具类：W3cDomTemplete
id: 1088
categories:
  - Java
date: 2013-08-14 01:49:48
tags:
---

<!--more-->
```java
package com.cnbool.util;

import java.io.IOException;
import java.io.InputStream;
import java.lang.reflect.Field;
import java.util.ArrayList;
import java.util.List;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.parsers.ParserConfigurationException;

import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;
import org.xml.sax.SAXException;

/**
 * 
 * @version	1.0
 *
 */
public class W3cDomTemplete {

	public interface NodeExtractor {
		public Object extractNode(Element node);
	}

	/**
	 * 从xml输入流中提取对象
	 * @param in	xml输入流
	 * @param nodeExtractor	对象提取器
	 * @return
	 * @throws SAXException
	 * @throws IOException
	 * @throws ParserConfigurationException
	 */
	public static Object extract(InputStream in, NodeExtractor nodeExtractor) throws SAXException, IOException, ParserConfigurationException {
		return nodeExtractor.extractNode(getRoot(in));
	}

	/**
	 * 从xml输入流中提取对象
	 * @param in	xml输入流
	 * @param c
	 * @return
	 * @throws SAXException
	 * @throws IOException
	 * @throws ParserConfigurationException
	 * @throws InstantiationException
	 * @throws IllegalAccessException
	 */
	public static<T> T extract(InputStream in, Class<?> c) throws SAXException, IOException, ParserConfigurationException, InstantiationException, IllegalAccessException {
		Element root = getRoot(in);
		return extractOne(root, c);
	}

	/**
	 * 从xml输入流中提取对象列表
	 * @param in	xml输入流
	 * @param c
	 * @return
	 * @throws SAXException
	 * @throws IOException
	 * @throws ParserConfigurationException
	 * @throws InstantiationException
	 * @throws IllegalAccessException
	 */
	public static<T> List<T> extractList(InputStream in, Class<?> c) throws SAXException, IOException, ParserConfigurationException, InstantiationException, IllegalAccessException {
		Element root = getRoot(in);
		List<T> resultList = new ArrayList<T>();
		matchNodes(resultList, root.getChildNodes(), c);
		return resultList;
	}

	/**
	 * 从xml输入流中提取对象列表
	 * @param in	xml输入流
	 * @param tagName	对象对应的标签名称
	 * @param nodeExtractor	节点提取器
	 * @return
	 * @throws SAXException
	 * @throws IOException
	 * @throws ParserConfigurationException
	 */
	public static<T> List<T> extractList(InputStream in, String tagName, NodeExtractor nodeExtractor) throws SAXException, IOException, ParserConfigurationException {
		Element root = getRoot(in);
		NodeList nodes = root.getElementsByTagName(tagName);
		List<T> resultList = new ArrayList<T>(nodes.getLength());

		for (int i = 0; i < nodes.getLength(); i++) {
			Object row = nodeExtractor.extractNode((Element) nodes.item(i));
			resultList.add((T) row);
		}

		return resultList;
	}

	/**
	 * 匹配合适的节点
	 * @param resultList
	 * @param nodes
	 * @param c
	 * @throws InstantiationException
	 * @throws IllegalAccessException
	 */
	private static <T> void matchNodes(List<T> resultList, NodeList nodes, Class<?> c) throws InstantiationException, IllegalAccessException {
		for (int i = 0; i < nodes.getLength(); i++) {
			Node subNode = nodes.item(i);
			if (subNode.getNodeType() == Node.ELEMENT_NODE) {
				if (c.getSimpleName().equalsIgnoreCase(subNode.getNodeName())) {
					T obj = extractOne(subNode, c);
					resultList.add(obj);
				} else {
					matchNodes(resultList, subNode.getChildNodes(), c);
				}
			}
		}
	}

	/**
	 * 从xml节点中提取一个对象
	 * @param node
	 * @param c
	 * @return
	 * @throws InstantiationException
	 * @throws IllegalAccessException
	 */
	private static<T> T extractOne(Node node, Class<?> c) throws InstantiationException, IllegalAccessException {
		Object obj = c.newInstance();
		Field[] fields = c.getDeclaredFields();

		if (node != null && c.getSimpleName().equalsIgnoreCase(node.getNodeName())) {
			NodeList nodelist = node.getChildNodes();

			for (int i = 0; i < nodelist.getLength(); i++) {
				Node subNode = nodelist.item(i);

				if (subNode.getNodeType() == Node.ELEMENT_NODE && subNode.getFirstChild().getNodeType() == Node.TEXT_NODE) {
					for (int j = 0; j < fields.length; j++) {
						if (fields[j].getName().equalsIgnoreCase(subNode.getNodeName())) {
							fields[j].setAccessible(true);
							fields[j].set(obj, subNode.getFirstChild().getNodeValue());
						}
					}
				}
			}
		}

		return (T) obj;
	}

	/**
	 * 从xml输入流中取得根节点
	 * @param in	xml输入流
	 * @return
	 * @throws SAXException
	 * @throws IOException
	 * @throws ParserConfigurationException
	 */
	private static Element getRoot(InputStream in) throws SAXException, IOException, ParserConfigurationException {
		DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
		DocumentBuilder builder = factory.newDocumentBuilder();
		Document document = builder.parse(in); 
		return document.getDocumentElement();
	}
}
```

**如何使用:**
**
Student:**
```java
public class Student {
	private String name;
	private String num;
	private String classes;
	private String address;
	private String tel;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getNum() {
		return num;
	}

	public void setNum(String num) {
		this.num = num;
	}

	public String getClasses() {
		return classes;
	}

	public void setClasses(String classes) {
		this.classes = classes;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public String getTel() {
		return tel;
	}

	public void setTel(String tel) {
		this.tel = tel;
	}

}

```

**
d:/test/Student.xml:**
[code lang="xml"]
<?xml version = "1.0" encoding = "gb2312"?>
<Student>
        <Name>小明</Name>
        <Num>1006010033</Num>
        <Classes>信管1</Classes>
        <Address>浙江杭州</Address>
        <Tel>18600001111</Tel>
</Student>
```

**提取Student对象:**
```java
//Method 1:
Student student = W3cDomTemplete.extract(getInputStream("d:/test/Student.xml"), Student.class);

//Method 2:
Student student = (Student) W3cDomTemplete.extract(getInputStream("d:/test/Student.xml"), new NodeExtractor() {

			@Override
			public Object extractNode(Element node) {
				Student student = new Student();
				student.setName(node.getElementsByTagName("Name").item(0).getFirstChild().getNodeValue());
				student.setNum(node.getElementsByTagName("Num").item(0).getFirstChild().getNodeValue());
				student.setClasses(node.getElementsByTagName("Classes").item(0).getFirstChild().getNodeValue());
				student.setAddress(node.getElementsByTagName("Address").item(0).getFirstChild().getNodeValue());
				student.setTel(node.getElementsByTagName("Tel").item(0).getFirstChild().getNodeValue());
				return student;
			}
		});

```

**
d:/test/School.xml:**
[code lang="xml"]
<?xml version = "1.0" encoding = "gb2312"?>
<School>
	<StuFar>
		<Student>
			<Name>沈1</Name>
			<Num>1006010022</Num>
			<Classes>信管</Classes>
			<Address>浙江杭州</Address>
			<Tel>123456</Tel>
		</Student>
		<Student>
			<Name>沈2</Name>
			<Num>1006010033</Num>
			<Classes>信管</Classes>
			<Address>浙江杭州</Address>
			<Tel>234567</Tel>
		</Student>
		<Student>
			<Name>沈3</Name>
			<Num>1006010044</Num>
			<Classes>生工</Classes>
			<Address>浙江杭州</Address>
			<Tel>345678</Tel>
		</Student>
		<Student>
			<Name>沈4</Name>
			<Num>1006010055</Num>
			<Classes>电子</Classes>
			<Address>浙江杭州</Address>
			<Tel>456789</Tel>
		</Student>
		<Lalala>
			<Student>
				<Name>沈5</Name>
				<Num>124234</Num>
				<Classes>生工</Classes>
				<Address>浙江杭州</Address>
				<Tel>345678</Tel>
			</Student>
			<Student>
				<Name>沈6</Name>
				<Num>23545</Num>
				<Classes>电子</Classes>
				<Address>浙江杭州</Address>
				<Tel>456789</Tel>
			</Student>
		</Lalala>
	</StuFar>
</School>
```

**提取Student集合:**
```java
//Method 1:
List<Student> studentList =  W3cDomTemplete.extractList(getInputStream("d:/test/School.xml"), Student.class);

//Method 2:
List<Student> studentList =  W3cDomTemplete.extractList(getInputStream("d:/test/School.xml"), "Student", new NodeExtractor() {

				@Override
				public Object extractNode(Element node) {
					Student student = new Student();
					student.setName(node.getElementsByTagName("Name").item(0).getFirstChild().getNodeValue());
					student.setNum(node.getElementsByTagName("Num").item(0).getFirstChild().getNodeValue());
					student.setClasses(node.getElementsByTagName("Classes").item(0).getFirstChild().getNodeValue());
					student.setAddress(node.getElementsByTagName("Address").item(0).getFirstChild().getNodeValue());
					student.setTel(node.getElementsByTagName("Tel").item(0).getFirstChild().getNodeValue());
					return student;
				}
			});
```
