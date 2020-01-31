## 4sharedSDK

`Download:`[https://github.com/jackkoolage/4sharedSDK/releases](https://github.com/jackkoolage/4sharedSDK/releases)<br>
`NuGet:`
[![NuGet](https://img.shields.io/nuget/v/DeQmaTech.4sharedSDK.svg?style=flat-square&logo=nuget)](https://www.nuget.org/packages/DeQmaTech.4sharedSDK)<br>


# Features:
* Assemblies for .NET 4.5.2 and .NET Standard 2.0 and .NET Core 2.1
* Just one external reference (Newtonsoft.Json)
* Easy installation using NuGet
* Upload/Download tracking support
* Proxy Support
* Upload/Download cancellation support


# List of functions:
**Token**
1. Login
1. Register

**Account(Mine)**
1. UserInfo
1. UpdateUserInfo
1. GetAvatarPicture
1. SetAvatarPicture

**Comment**
1. ListComments
1. Create
1. Delete
1. Reply
1. ReportAsSpam
1. UnReportAsSpam

**Data**
1. SearchRoot
1. EmptyRecycleBin
1. ListRoot
1. RootID
1. RecycleBinID
1. ListRecycleBin
1. RestoreRecycleBin
1. GetLinkForFile

**File**
1. Upload
1. UploadLargeFile
1. Update
1. UpdateLargeFile
1. Copy
1. CopyMultiple
1. Move
1. MoveMultiple
1. Trash
1. UnTrash
1. TrashMultiple
1. Delete
1. DeleteMultiple
1. Metadata
1. EditMetadata
1. ChangePrivacy
1. Rename
1. Download
1. DownloadLargeFile
1. DownloadAsStream
1. ImportToMyAccount
1. ThumbnailUrl
1. UnZip
1. Exists

**Folder**
1. Password
1. SetDescription
1. ChangePermission
1. ChangePrivacy
1. Search
1. ListSubFiles
1. ListSubFolders
1. Create
1. Copy
1. CopyMultiple
1. Move
1. MoveMultiple
1. UnTrash
1. Trash
1. Delete
1. DeleteMultiple
1. TrashMultiple
1. Rename
1. Metadata
1. Exists
1. ExistsInTargetFolder

**User**
1. SearchFiles
1. Metadata
1. AvatarPicture
1. ListFilesRelatedToFile


# CodeMap:
![codemap](https://i.postimg.cc/yNZ3JPGW/4s-codemap.png)


# Code simple:
```vb
    Async Function tasks() As Task
        'first login (one time only)
        Dim tokn = Await FourSharedSDK.GetToken.Login_Base64("your_email", "your_password")
        ''set proxy and connection options
        Dim con As New FourSharedSDK.ConnectionSettings With {.CloseConnection = True, .TimeOut = TimeSpan.FromMinutes(30), .Proxy = New FourSharedSDK.ProxyConfig With {.SetProxy = True, .ProxyIP = "127.0.0.1", .ProxyPort = 8888, .ProxyUsername = "user", .ProxyPassword = "pass"}}
        ''set api client
        Dim client As FourSharedSDK.IClient = New FourSharedSDK.FClient(tokn, Nothing)


        ''functions:
        'Account
        Await client.Account.GetAvatarPicture(ImageSizeEnum._72x72)
        Dim _ReportCls As New Progress(Of FourSharedSDK.ReportStatus)(Sub(ReportClass As FourSharedSDK.ReportStatus)
                                                                          Label1.Text = String.Format("{0}/{1}", (ReportClass.BytesTransferred), (ReportClass.TotalBytes))
                                                                          Label1.Text = CInt(ReportClass.ProgressPercentage)
                                                                          Label1.Text = If(CStr(ReportClass.TextStatus) Is Nothing, "Uploading...", CStr(ReportClass.TextStatus))
                                                                      End Sub)
        Await client.Account.SetAvatarPicture("c:\\pic.jpg", UploadTypes.FilePath, "pic.jpg", _ReportCls, Nothing)
        Await client.Account.UpdateUserInfo("first_name", "last_name", "pass", "email", AllowSearchEnum.disabled)
        Await client.Account.UserInfo

        ''Comments
        Await client.Comments.Create("file_id", "this is comment")
        Await client.Comments.Delete("file_id", "comment_id")
        Await client.Comments.ListComments("file_id", 50, 0)
        Await client.Comments.Reply("file_id", "comment_id", "this is replay to an comment")
        Await client.Comments.ReportAsSpam("comment_id")
        Await client.Comments.UnReportAsSpam("comment_id")

        ''Data
        Await client.Data.EmptyRecycleBin
        Await client.Data.GetLinkForFile
        Await client.Data.ListRecycleBin(Nothing, 50, 0)
        Await client.Data.ListRoot("root_id", 50, 0)
        Await client.Data.RecycleBinID("root_id")
        Await client.Data.RestoreRecycleBin(Nothing)
        Await client.Data.RootID()
        Await client.Data.SearchRoot("emy", FileTypeEnum.Music, 50, 0)

        ''Files
        Await client.Files.ChangePrivacy("", True)
        Await client.Files.Copy("file_id", "folder_id", "newName")
        Await client.Files.CopyMultiple(New List(Of String) From {{"file_id"}, {"file_id"}}, "folder_id")
        Await client.Files.Delete("file_id")
        Await client.Files.DeleteMultiple(New List(Of String) From {{"file_id"}, {"file_id"}})
        Await client.Files.Download("/My 4shared/oppo/bouncy1.8.5.zip", "c:\\", _ReportCls, Nothing)
        Await client.Files.DownloadAsStream("/My 4shared/oppo/bouncy1.8.5.zip", _ReportCls, Nothing)
        Await client.Files.DownloadLargeFile("/My 4shared/oppo/bouncy1.8.5.zip", "c:\\", _ReportCls, Nothing)
        Await client.Files.EditMetadata("file_id", "this is file", "sdk,api,cloud")
        Await client.Files.Exists("file_id")
        Await client.Files.ImportToMyAccount("file_id", "folder_id")
        Await client.Files.Metadata("file_id", AddFieldsEnum.ImageMetainfo)
        Await client.Files.Move("file_id", "folder_id")
        Await client.Files.MoveMultiple(New List(Of String) From {{"file_id"}, {"file_id"}}, "folder_id")
        Await client.Files.Rename("file_id", "newName")
        Await client.Files.ThumbnailUrl("file_id", ImageSizeEnum._320x240)
        Await client.Files.Trash("file_id")
        Await client.Files.TrashMultiple(New List(Of String) From {{"file_id"}, {"file_id"}})
        Await client.Files.UnTrash("file_id")
        Await client.Files.UnZip("file_id")
        Await client.Files.Update("c:\\fle.mp4", UploadTypes.FilePath, "file_id", "folder_id", "fle.mp4", _ReportCls, Nothing)
        Await client.Files.UpdateLargeFile("c:\\fle.mp4", UploadTypes.FilePath, "file_id", "folder_id", "fle.mp4", 12345, _ReportCls, Nothing)
        Await client.Files.Upload("c:\\fle.mp4", UploadTypes.FilePath, "folder_id", "fle.mp4", _ReportCls, Nothing)
        Await client.Files.UploadLargeFile("c:\\fle.mp4", UploadTypes.FilePath, "folder_id", "fle.mp4", 123456, _ReportCls, Nothing)

        ''Folders
        Await client.Folders.ChangePermission("folder_id", FolderPermissionsEnum.read)
        Await client.Folders.ChangePrivacy("", PrivacyEnum.private)
        Await client.Folders.Copy("folder_id", "folder_id", Nothing)
        Await client.Folders.CopyMultiple(New List(Of String) From {{"folder_id"}, {"folder_id"}}, "folder_id")
        Await client.Folders.Create("folder_id", "new folder", "music dir")
        Await client.Folders.Delete("folder_id")
        Await client.Folders.DeleteMultiple(New List(Of String) From {{"folder_id"}, {"folder_id"}})
        Await client.Folders.Exists("folder_id")
        Await client.Folders.ExistsInTargetFolder("work folder", "folder_id")
        Await client.Folders.ListSubFiles("folder_id", FileTypeEnum.Video, AddFieldsEnum.id3, 50, 0)
        Await client.Folders.ListSubFolders("folder_id", 50, 0)
        Await client.Folders.Metadata("folder_id")
        Await client.Folders.Move("folder_id", "folder_id", Nothing)
        Await client.Folders.MoveMultiple(New List(Of String) From {{"folder_id"}, {"folder_id"}}, "folder_id")
        Await client.Folders.Password("folder_id", "1234")
        Await client.Folders.Rename("folder_id", "new name")
        Await client.Folders.Search("folder_id", "emy", FileTypeEnum.Video, 50, 0)
        Await client.Folders.SetDescription("folder_id", "this is des")
        Await client.Folders.Trash("folder_id")
        Await client.Folders.TrashMultiple(New List(Of String) From {{"folder_id"}, {"folder_id"}})
        Await client.Folders.UnTrash("folder_id")

        ''Users
        Await client.Users.AvatarPicture("user_id", ImageSizeEnum._320x240)
        Await client.Users.ListFilesRelatedToFile("file_id", 50, 0)
        Await client.Users.Metadata("user_id")
        Await client.Users.SearchFiles("emy", FilesFilterEnum.all, FilesTypeEnum.asf, 5000, 60000, SortByEnum.name, OrderByEnum.asc, 50, 0)
    End Function
    ```
