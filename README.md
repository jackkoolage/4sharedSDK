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
'first login (one time only)

    Async Sub Login_Base64()
        Dim lgnBase64 = Await FourSharedSDK.GetToken.Login_Base64("JackKoolage@go.com", "123456")
        PropertyGrid1.SelectedObject = (lgnBase64)
    End Sub

'Set your client
    Dim Clnt As FourSharedSDK.IClient = New FourSharedSDK.FClient(lgnBase64, Nothing)



'Functions:
Imports FourSharedSDK.utilitiez

    Async Sub UserInfo()
        Dim rslt = Await Clnt.Account.UserInfo()
        PropertyGrid1.SelectedObject = rslt
    End Sub

    Async Sub UpdateUserInfo()
        Dim rslt = Await Clnt.Account.UpdateUserInfo(Nothing, Nothing, Nothing, Nothing, AllowSearchEnum.disabled)
        DataGridView1.Rows.Add(rslt.allowSearch.ToString)
    End Sub

    Async Sub GetAvatarPicture()
        Dim rslt = Await Clnt.Account.AvatarPicture(ImageSizeEnum._72x72)
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub SetAvatarPicture()
        Dim rslt = Await Clnt.Account.SetAvatarPicture("C:\1e.jpg", UploadTypes.FilePath, "1e.jpg", Nothing, Nothing)
        MsgBox(rslt)
    End Sub

    Async Sub LisSubtFiles()
        Dim rslt = Await Clnt.Folders.ListSubFiles("Dir_id", Nothing, 100, 0)
        DataGridView1.Rows.Add("name", "IsPrivate", "id", "size", "thumbnailUrl", "parentId", "downloadUrl", "ImgUrl")
        For Each onz In rslt.files
            DataGridView1.Rows.Add(onz.name, onz.IsPrivate, onz.id, onz.size, onz.thumbnailUrl, onz.parentId, onz.downloadUrl, onz.ImgUrl(ImageSizeEnum._320x240))
        Next
    End Sub

    Async Sub ListSubFolders()
        Dim rslt = Await Clnt.Folders.ListSubFolders("Dir_id", 100, 0)
        DataGridView1.Rows.Add("name", "Privacy", "id", "TotalFiles", "TotalFolders", "parentId", "permissions", "folderLink")
        For Each onz In rslt.folders
            DataGridView1.Rows.Add(onz.name, onz.Privacy.ToString, onz.id, onz.TotalFiles, onz.TotalFolders, onz.parentId, onz.permissions.ToString, onz.folderLink)
        Next
    End Sub

    Async Sub Search()
        Dim rslt = Await Clnt.Folders.Search("Dir_id", TextBox_searchKeyword.Text, Nothing, 100, 0)
        DataGridView1.Rows.Add("name", "IsPrivate", "id", "size", "thumbnailUrl", "parentId", "downloadUrl", "ImgUrl")
        For Each onz In rslt.files
            DataGridView1.Rows.Add(onz.name, onz.IsPrivate, onz.id, ConvSze(onz.size), onz.thumbnailUrl, onz.parentId, onz.downloadUrl, onz.ImgUrl(ImageSizeEnum._320x240))
        Next
    End Sub

    Async Sub SearchRoot()
        Dim rslt = Await Clnt.Data.SearchRoot(TextBox_searchKeyword.Text, Nothing, 100, 0)
        DataGridView1.Rows.Add("name", "IsPrivate", "id", "size", "thumbnailUrl", "parentId", "downloadUrl", "ImgUrl")
        For Each onz In rslt.files
            DataGridView1.Rows.Add(onz.name, onz.IsPrivate, onz.id, ConvSze(onz.size), onz.thumbnailUrl, onz.parentId, onz.downloadUrl, onz.ImgUrl(ImageSizeEnum._320x240))
        Next
    End Sub

    Async Sub Create()
        Dim onz = Await Clnt.Folders.Create("Dir_id", "lolo", "this is a new folder of")
        DataGridView1.Rows.Add("name", "Privacy", "id", "TotalFiles", "TotalFolders", "parentId", "permissions", "folderLink")
        DataGridView1.Rows.Add(onz.name, onz.Privacy.ToString, onz.id, onz.TotalFiles, onz.TotalFolders, onz.parentId, onz.permissions.ToString, onz.folderLink)
    End Sub

    Async Sub Rename()
        Dim onz = Await Clnt.Folders.Rename("Dir_id", "MyNewName")
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub ChangePermission()
        Dim onz = Await Clnt.Folders.ChangePermission("Dir_id", FolderPermissionsEnum.read)
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub ChangePrivacy()
        Dim onz = Await Clnt.Folders.ChangePrivacy("Dir_id", PrivacyEnum.private)
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub Password()
        Dim onz = Await Clnt.Folders.Password("Dir_id", "1905")
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub FolderMetadata()
        Dim onz = Await Clnt.Folders.Metadata("Dir_id")
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub FileMetadata()
        Dim onz = Await Clnt.Files.Metadata("File_id", AddFieldsEnum.ImageMetainfo)
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub Exists()
        Dim onz = Await Clnt.Folders.Exists("Dir_id")
        MsgBox(onz)
    End Sub

    Async Sub SetDescription()
        Dim onz = Await Clnt.Folders.SetDescription("Dir_id", "look at this")
        PropertyGrid1.SelectedObject = onz
    End Sub

    Async Sub ListRoot()
        Dim getRootId= Await Clnt.Account.UserInfo()
        Dim rslt = Await Clnt.Data.ListRoot(getRootId.rootFolderId, 100, 0)
        PropertyGrid1.SelectedObject = rslt.CurrentFolderOptions.Settings
        DataGridView1.Rows.Add(rslt.FilesFoldersList.UsedSpace, rslt.FilesFoldersList.DailyTrafficUsage)

        For Each onz In rslt.FilesFoldersList.FoldersList
            DataGridView1.Rows.Add(onz.name, onz.Privacy.ToString, onz.id, ConvSze(onz.size), onz.CreatedDate)
        Next
        For Each onz In rslt.FilesFoldersList.FilesList
            DataGridView1.Rows.Add(onz.name, onz.canPreview, onz.id, ConvSze(onz.size), onz.ThumbUrl, onz.CreatedDate, onz.Ext, onz.typeCss)
        Next
    End Sub

    Async Sub UsersSearchFiles()
        Dim rslt = Await Clnt.Users.SearchFiles("big", FileTypeEnum.Archives, Nothing, Nothing, Nothing, SortByEnum.size, OrderByEnum.asc, 100, 0)

        For Each onz In rslt.items
            DataGridView1.Rows.Add(onz.fileName, onz.fileId, onz.fileUrl, ConvSze(onz.fileSize), onz.dirName, onz.dirId, onz.dirPath, onz.category, onz.thumbnailUrl)
        Next
        L_Counter.Text = rslt.items.Count
    End Sub

    Async Sub Upload()
        Dim frm As New DeQma.FileFolderDialogs.VistaOpenFileDialog With {.Multiselect = False}
        If frm.ShowDialog = DialogResult.OK AndAlso Not String.IsNullOrEmpty(frm.FileName) Then

            Dim UploadCancellationToken As New Threading.CancellationTokenSource()
            Dim _ReportCls As New Progress(Of FourSharedSDK.ReportStatus)(Sub(ReportClass As FourSharedSDK.ReportStatus)
                                                                              Label1.Text = String.Format("{0}/{1}", ConvSze(ReportClass.BytesTransferred), ConvSze(ReportClass.TotalBytes))
                                                                              ProgressBar1.Value = CInt(ReportClass.ProgressPercentage)
                                                                              Label2.Text = If(CStr(ReportClass.TextStatus) Is Nothing, "Uploading...", CStr(ReportClass.TextStatus))
                                                                          End Sub)
            Dim rslt = Await Clnt.Files.Upload(frm.FileName, UploadTypes.FilePath, "Dir_id", IO.Path.GetFileName(frm.FileName), _ReportCls, UploadCancellationToken.Token)
            PropertyGrid1.SelectedObject = rslt
        End If
    End Sub

    Async Sub UploadLargeFile()
            Dim sze = New IO.FileInfo(frm.FileName).Length
            Dim rslt = Await Clnt.Files.UploadLargeFile(frm.FileName, UploadTypes.FilePath, "Dir_id", IO.Path.GetFileName(frm.FileName), sze, _ReportCls, UploadCancellationToken.Token)
    End Sub

    Async Sub FileUpdate()
            Dim rslt = Await Clnt.Files.Update(frm.FileName, UploadTypes.FilePath, "File_id", "Dir_id", IO.Path.GetFileName(frm.FileName), _ReportCls, UploadCancellationToken.Token)
            PropertyGrid1.SelectedObject = rslt
    End Sub

    Async Sub ThumbnailUrl()
        Dim onz = Await Clnt.Files.ThumbnailUrl("File_id", ImageSizeEnum._320x240)
        DataGridView1.Rows.Add(onz)
    End Sub

    Async Sub DeleteMultipleFiles()
        Dim onz = Await Clnt.Files.DeleteMultiple(New List(Of String) From {"File_id_1", "File_id_2", "File_id_3"})
        DataGridView1.Rows.Add(onz)
    End Sub

    Async Sub Download()
        Dim frm As New DeQma.FileFolderDialogs.VistaFolderBrowserDialog With {.ShowNewFolderButton = True}
        If frm.ShowDialog = DialogResult.OK AndAlso Not String.IsNullOrEmpty(frm.SelectedPath) Then
            Dim UploadCancellationToken As New Threading.CancellationTokenSource()
            Dim _ReportCls As New Progress(Of FourSharedSDK.ReportStatus)(Sub(ReportClass As FourSharedSDK.ReportStatus)
                                                                              Label1.Text = String.Format("{0}/{1}", ConvSze(ReportClass.BytesTransferred), ConvSze(ReportClass.TotalBytes))
                                                                              ProgressBar1.Value = CInt(ReportClass.ProgressPercentage)
                                                                              Label2.Text = If(CStr(ReportClass.TextStatus) Is Nothing, "Downloading...", CStr(ReportClass.TextStatus))
                                                                          End Sub)
            Dim getFileId= Await Clnt.Files.Metadata("File_id")
            Await Clnt.Files.Download(getFileId.path, frm.SelectedPath, _ReportCls, UploadCancellationToken.Token)
        End If
    End Sub

    Async Sub UsersMetadata()
        Dim rslt = Await Clnt.Users.Metadata("User_id")
        PropertyGrid1.SelectedObject = rslt
    End Sub

    Async Sub UsersAvatar()
        Dim rslt = Await Clnt.Users.AvatarPicture("User_id", ImageSizeEnum._2560x1600)
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub ExistsInDir()
        Dim rslt = Await Clnt.Folders.ExistsInTargetFolder("New%20folder", "Dir_id")
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub TrashMultipleFolders()
        Dim onz = Await Clnt.Folders.TrashMultiple(New List(Of String) From {"Dir_id_1", "Dir_id_2"})
        DataGridView1.Rows.Add(onz)
    End Sub

    Async Sub ListComments()
        Dim rslt = Await Clnt.Comments.ListComments("File_id", 100, 0)

        For Each onz In rslt.comments
            DataGridView1.Rows.Add(onz.Comment, onz.id, onz.User.userName)
        Next
    End Sub

    Async Sub Comment()
        Dim rslt = Await Clnt.Comments.Create("File_id", "test api")
        PropertyGrid1.SelectedObject = rslt
    End Sub

    Async Sub CommentDelete()
        Dim rslt = Await Clnt.Comments.Delete("File_id", "Comment_id")
        MsgBox(rslt)
    End Sub

    Async Sub ReplyComment()
        Dim rslt = Await Clnt.Comments.Reply("File_id", "Comment_id", "testing message...")
        PropertyGrid1.SelectedObject = rslt
    End Sub

    Async Sub ReportAsSpam_UnReportAsSpam()
        Dim reporting = Await Clnt.Comments.ReportAsSpam("Comment_id")
        Dim rslt2 = Await Clnt.Comments.UnReportAsSpam(reporting)
    End Sub

    Async Sub ListFilesRelatedToFile()
        Dim rslt = Await Clnt.Users.ListFilesRelatedToFile("File_id", 100, 0)

        For Each onz In rslt.files
            DataGridView1.Rows.Add(onz.name, onz.IsPrivate, onz.id, ConvSze(onz.size), onz.thumbnailUrl, onz.parentId, onz.downloadUrl, onz.ImgUrl(ImageSizeEnum._320x240))
        Next
    End Sub

    Async Sub ImportToMyAccount()
        Dim rslt = Await Clnt.Files.ImportToMyAccount("File_id", "ImportToDir_id")
        PropertyGrid1.SelectedObject = rslt
    End Sub

    Async Sub RootID()
        Dim rslt = Await Clnt.Data.RootID()
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub EmptyRecycleBin()
        Dim rslt = Await Clnt.Data.EmptyRecycleBin()
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub RecycleBinID()
        'Dim rslt = Await Clnt.Data.RecycleBinID()
        Dim rslt = Await Clnt.Data.RecycleBinID("Root_id")
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub ListRecycleBin()
        Dim recBinId = Await Clnt.Data.RecycleBinID
        Dim rslt = Await Clnt.Data.ListRecycleBin(recBinId, 100, 0)

        For Each onz In rslt.files
            DataGridView1.Rows.Add(onz.name, onz.time, onz.id, ConvSze(onz.size), onz.recoverPath)
        Next
        For Each onz In rslt.dirs
            DataGridView1.Rows.Add(onz.name, onz.time, onz.id, ConvSze(onz.size), onz.recoverPath)
        Next
    End Sub

    Async Sub RestoreRecycleBin()
        Dim recBinId = Await Clnt.Data.RecycleBinID
        Dim rslt = Await Clnt.Data.RestoreRecycleBin(recBinId)
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub GetLinkForFile()
        Dim rslt = Await Clnt.Data.GetLinkForFile()
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub FileUnZip()
        Dim rslt = Await Clnt.Files.UnZip("File_id")
        DataGridView1.Rows.Add(rslt)
    End Sub

    Async Sub FileExists()
        Dim rslt = Await Clnt.Files.Exists("File_id")
        DataGridView1.Rows.Add(rslt)
    End Sub






```
