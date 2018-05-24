title: FileProvider
author: 大帅
tags:
  - FileProvider
categories:
  - Android
date: 2018-05-24 17:42:00
---
为了给应用程序定义一个FileProvider，需要在Manifest清单文件中定义一个entry，该entry指明了需要使用的创建Content URI的Authority。此外，还需要一个XML文件的文件名，该XML文件指定了我们的应用可以共享的目录路径。

下例展示了如何在清单文件中添加<provider>标签，来指定FileProvider类，Authority及XML文件名：
  		
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <application
        ...>
        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="com.example.myapp.fileprovider"
            android:grantUriPermissions="true"
            android:exported="false">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/filepaths" />
        </provider>
        ...
    </application>
	</manifest>
在“res/xml/”下创建文件“filepaths.xml”。在这个文件中，为每一个想要共享目录添加一个XML标签
		
    <paths>
    	<files-path path="images/" name="myimages" />
	</paths>
    
<files-path>标签共享的是在我们应用的内部存储中“files/”目录下的目录。“path”属性字段指出了该子目录为“files/”目录下的子目录“images/”。“name”属性字段告知FileProvider在“files/images/”子目录中的文件的Content URI添加路径分段（path segment）标记：“myimages”。

<paths>标签可以有多个子标签，每一个子标签用来指定不同的共享目录。除了<files-path>标签，还可以使用<external-path>来共享位于外部存储的目录；另外，<cache-path>标签用来共享在内部缓存目录下的子目录。   
  在文件分享是时调用给与临时的权限：
  ``Intent.addFlags(
                       Intent.FLAG_GRANT_READ_URI_PERMISSION);``
