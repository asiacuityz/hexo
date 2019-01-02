title: XML工具类
author: Asia Cui
tags:
  - Java工具类
categories:
  - XML工具类
date: 2019-01-02 13:15:00
---
## Features

* 将 Object 转换为 xml 格式字符串
* 将 xml 格式的 String 字符串转换为 Object

```java
/**
 * xml工具类
 * @author AsiaCui
 */
import java.io.StringReader;
import java.io.StringWriter;

import javax.xml.bind.JAXBContext;
import javax.xml.bind.Marshaller;
import javax.xml.bind.Unmarshaller;

public class XMLUtil {

    /**
     * 将javabean转换为xml格式字符串
     * @param obj
     * @return
     */
     public static String toXmlString(Object obj) {
            String result;
            try {
                JAXBContext context = JAXBContext.newInstance(obj.getClass());
                Marshaller marshaller = context.createMarshaller();
                StringWriter writer = new StringWriter();
                marshaller.marshal(obj, writer);
                result = writer.toString();
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
            return result;
        }

     /**
      * 将xml格式的String字符串转换为javabeen
      * @param input xml格式字符串
      * @param claaz
      * @return
      */
        @SuppressWarnings("unchecked")
        public static <T> T parseObject(String input, Class<T> claaz) {
            Object result;
            try {
                JAXBContext context = JAXBContext.newInstance(claaz);
                Unmarshaller unmarshaller = context.createUnmarshaller();
                result = unmarshaller.unmarshal(new StringReader(input));
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
            return (T) result;
        }

}

```