---
title: "如何使用 Java 的檔案儲存體 | Microsoft Docs"
description: "了解如何使用 Azure 檔案服務來上傳、下載、列出及刪除檔案。 範例以 Java 撰寫。"
services: storage
documentationcenter: java
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 3bfbfa7f-d378-4fb4-8df3-e0b6fcea5b27
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 60d4bcafa51a6eedea06901a0557d13b4e85ec7a


---
# <a name="how-to-use-file-storage-from-java"></a>如何使用 Java 的檔案儲存體
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="overview"></a>Overview
在本指南中，您將學習如何對 Microsoft Azure 檔案儲存體服務執行基本作業。 透過以 Java 撰寫的範例，您將學習如何建立共用和目錄、上傳、清單，及刪除檔案。 如果您是 Microsoft Azure 檔案儲存體服務的新手，透過下列各節中的概念對於了解範例很有幫助。

[!INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>建立 Java 應用程式
如要建置範例，您將需要 Java Development Kit (JDK) 和 適用於 Java 的 Azure 儲存體 SDK。 您也應該建立 Azure 儲存體帳戶。

## <a name="setup-your-application-to-use-file-storage"></a>設定您的應用程式以使用檔案儲存體
若要使用 Azure 儲存體 API，請將下列陳述式加入至您要從其存取儲存體服務的 Java 檔案頂端。

```java
// Include the following imports to use blob APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.file.*;
```

## <a name="setup-an-azure-storage-connection-string"></a>設定 Azure 儲存體連接字串
若要使用檔案儲存體，您必須連接到您的 Azure 儲存體帳戶。 第一個步驟是設定連接字串，我們會用來連接到您的儲存體帳戶。 讓我們定義靜態變數以便進行。

```java
// Configure the connection-string with your values
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account_name;" +
    "AccountKey=your_storage_account_key";
```

> [!NOTE]
> 將 your_storage_account_name 和 your_storage_account_key 取代為您的儲存體帳戶的實際值。
> 
> 

## <a name="connecting-to-an-azure-storage-account"></a>連接到 Azure 儲存體帳戶
若要連接到您的儲存體帳戶，您必須使用 **CloudStorageAccount** 物件，將連接字串傳遞至其**剖析**方法。

```java
// Use the CloudStorageAccount object to connect to your storage account
try {
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
} catch (InvalidKeyException invalidKey) {
    // Handle the exception
}
```

**CloudStorageAccount.parse** 會擲回 InvalidKeyException，因此您必須將其放在 try/catch 區塊內。

## <a name="how-to-create-a-share"></a>如何：建立共用
檔案儲存體的所有檔案和目錄都位於名為 [ **共用**] 的容器中。 您的儲存體帳戶可以有帳戶容量允許數量的共用。 若要取得共用及其內容的存取權，您必須使用檔案儲存體用戶端。

```java
// Create the file storage client.
CloudFileClient fileClient = storageAccount.createCloudFileClient();
```

使用檔案儲存體用戶端，您可以取得共用的參考。

```java
// Get a reference to the file share
CloudFileShare share = fileClient.getShareReference("sampleshare");
```

若要實際建立共用，請使用 CloudFileShare 物件的 **createIfNotExists** 方法。

```java
if (share.createIfNotExists()) {
    System.out.println("New share created");
}
```

目前，**共用**會將參考保留至名為 **sampleshare** 的共用。

## <a name="how-to-upload-a-file"></a>如何：上傳檔案
Azure 檔案儲存體共用至少包含根目錄，檔案可以放置其中。 在本節中，您將學習如何從本機儲存體將檔案上傳至共用的根目錄。

上傳檔案的第一個步驟是取得檔案所在之目錄的參考。 您可以藉由呼叫共用物件的 **getRootDirectoryReference** 方法來完成。

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();
```

現在您具有共用之根目錄的參考，您可以使用下列程式碼上傳檔案。

```java
        // Define the path to a local file.
        final String filePath = "C:\\temp\\Readme.txt";
    
        CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
        cloudFile.uploadFromFile(filePath);
```

## <a name="how-to-create-a-directory"></a>如何：建立目錄
您也可以組織儲存體，方法是將檔案放在子目錄中，而不是將所有檔案都放在根目錄中。 Azure 檔案儲存體服務可讓您建立您的帳戶允許數量的目錄。 下列程式碼會在根目錄底下建立名為 **sampledir** 的子目錄。

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the sampledir directory
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

if (sampleDir.createIfNotExists()) {
    System.out.println("sampledir created");
} else {
    System.out.println("sampledir already exists");
}
```

## <a name="how-to-list-files-and-directories-in-a-share"></a>如何：列出共用中的檔案和目錄
取得共用內檔案和目錄的清單很容易，只要在 CloudFileDirectory 參考上呼叫 **listFilesAndDirectories** 即可。 方法會傳回您可以逐一查看的 ListFileItem 物件清單。 例如，下列程式碼會列出根目錄內的檔案和目錄。

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
    System.out.println(fileItem.getUri());
}
```

## <a name="how-to-download-a-file"></a>如何：下載檔案
您針對檔案儲存體執行的其中一個較頻繁作業是下載檔案。 在下列範例中，程式碼會下載 SampleFile.txt，並顯示其內容。

```java
//Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

//Get a reference to the directory that contains the file
CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

//Get a reference to the file you want to download
CloudFile file = sampleDir.getFileReference("SampleFile.txt");

//Write the contents of the file to the console.
System.out.println(file.downloadText());
```

## <a name="how-to-delete-a-file"></a>如何：刪除檔案
另一個常見的檔案儲存體作業是刪除檔案。 下列程式碼會刪除儲存在名為 **sampledir**之目錄內名為 SampleFile.txt 的檔案。

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory where the file to be deleted is in
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

String filename = "SampleFile.txt"
CloudFile file;

file = containerDir.getFileReference(filename)
if ( file.deleteIfExists() ) {
    System.out.println(filename + " was deleted");
}
```

## <a name="how-to-delete-a-directory"></a>如何：刪除目錄
刪除目錄是相當簡單的工作，不過請注意您無法刪除仍然包含檔案或其他目錄的目錄。

```java
// Get a reference to the root directory for the share.
CloudFileDirectory rootDir = share.getRootDirectoryReference();

// Get a reference to the directory you want to delete
CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

// Delete the directory
if ( containerDir.deleteIfExists() ) {
    System.out.println("Directory deleted");
}
```

## <a name="how-to-delete-a-share"></a>如何：刪除共用
刪除共用可以藉由在 CloudFileShare 物件呼叫 **deleteIfExists** 方法來完成。 以下是執行該作業的範例程式碼。

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

    // Create the file client.
   CloudFileClient fileClient = storageAccount.createCloudFileClient();

   // Get a reference to the file share
   CloudFileShare share = fileClient.getShareReference("sampleshare");

   if (share.deleteIfExists()) {
       System.out.println("sampleshare deleted");
   }
} catch (Exception e) {
    e.printStackTrace();
}
```

## <a name="next-steps"></a>後續步驟
如果您想要深入了解其他 Azure 儲存體 API，請參考下列連結。

* [Java 開發人員中心](http://azure.microsoft.com/develop/java/)
* [Azure Storage SDK for Java](https://github.com/azure/azure-storage-java)
* [Azure Storage SDK for Android](https://github.com/azure/azure-storage-android)
* [Azure 儲存體用戶端 SDK 參考](http://dl.windowsazure.com/storage/javadoc/)
* [Azure 儲存體服務 REST API](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Azure 儲存體團隊部落格](http://blogs.msdn.com/b/windowsazurestorage/)
* [使用 AzCopy 命令列公用程式傳輸資料](storage-use-azcopy.md)




<!--HONumber=Nov16_HO3-->


