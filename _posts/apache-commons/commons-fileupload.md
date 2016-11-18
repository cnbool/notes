title: Commons FileUpload
categories:
  - apache-commons
date: 2012-10-08 18:21:55
tags:
---

A component for handling HTML file uploads as specified by [RFC 1867](http://www.ietf.org/rfc/rfc1867.txt). This component provides support for uploads within both servlets (JSR 53) and portlets (JSR 168).

While this package provides the generic functionality for file uploads, these classes are not typically used directly. Instead, normal usage involves one of the provided extensions of [`FileUpload`](http://commons.apache.org/fileupload/apidocs/org/apache/commons/fileupload/FileUpload.html "class in org.apache.commons.fileupload") such as [`ServletFileUpload`](http://commons.apache.org/fileupload/apidocs/org/apache/commons/fileupload/servlet/ServletFileUpload.html "class in org.apache.commons.fileupload.servlet") or [`PortletFileUpload`](http://commons.apache.org/fileupload/apidocs/org/apache/commons/fileupload/portlet/PortletFileUpload.html "class in org.apache.commons.fileupload.portlet"), together with a factory for [`FileItem`](http://commons.apache.org/fileupload/apidocs/org/apache/commons/fileupload/FileItem.html "interface in org.apache.commons.fileupload") instances, such as [`DiskFileItemFactory`](http://commons.apache.org/fileupload/apidocs/org/apache/commons/fileupload/disk/DiskFileItemFactory.html "class in org.apache.commons.fileupload.disk").<!--more-->

The following is a brief example of typical usage in a servlet, storing the uploaded files on disk.

```java
    public void doPost(HttpServletRequest req, HttpServletResponse res) {
        DiskFileItemFactory factory = new DiskFileItemFactory();
        // maximum size that will be stored in memory
        factory.setSizeThreshold(4096);
        // the location for saving data that is larger than getSizeThreshold()
        factory.setRepository(new File("/tmp"));

        ServletFileUpload upload = new ServletFileUpload(factory);
        // maximum size before a FileUploadException will be thrown
        upload.setSizeMax(1000000);

        List fileItems = upload.parseRequest(req);
        // assume we know there are two files. The first file is a small
        // text file, the second is unknown and is written to a file on
        // the server
        Iterator i = fileItems.iterator();
        String comment = ((FileItem)i.next()).getString();
        FileItem fi = (FileItem)i.next();
        // filename on the client
        String fileName = fi.getName();
        // save comment and filename to database
        ...
        // write the file
        fi.write(new File("/www/uploads/", fileName));
    }
```

In the example above, the first file is loaded into memory as a `String`. Before calling the `getString` method, the data may have been in memory or on disk depending on its size. The second file we assume it will be large and therefore never explicitly load it into memory, though if it is less than 4096 bytes it will be in memory before it is written to its final location. When writing to the final location, if the data is larger than the threshold, an attempt is made to rename the temporary file to the given location. If it cannot be renamed, it is streamed to the new location.

Please see the FileUpload [User Guide](http://commons.apache.org/fileupload/using.html) for further details and examples of how to use this package.
