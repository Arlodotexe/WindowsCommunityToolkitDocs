---
title: Facebook Service 
author: nmetulev
description: The Facebook Service allows you to retrieve or publish data to the Facebook graph. Examples of the types of objects you can work with are Posts, Tagged Objects, and the primary user feed (outdated docs).
keywords: windows 10, uwp, windows community toolkit, uwp community toolkit, uwp toolkit, Facebook Service 
dev_langs:
  - csharp
  - vb
---

# Facebook Service

> [!WARNING]
> The Facebook Service is no longer available in the Windows Community Toolkit. The underlying dependent library was no longer maintained.

The Facebook Service allows you to retrieve or publish data to the Facebook graph. Examples of the types of objects you can work with are Posts, Tagged Objects, and the primary user feed.

## Getting Windows Store SID

The Windows Store SID is a unique value per application generated, and it not tied to the actual store publication.  Creating a local application will give you a valid SID that you can use for debugging against Facebook.  

```csharp
// Put the following code in your mainform loaded event
// Note that this will not work in the App.xaml.cs Loaded
#if DEBUG
    System.Diagnostics.Debug.WriteLine("Windows Store SID = " + Microsoft.Toolkit.Uwp.Services.Facebook.FacebookService.Instance.WindowsStoreId);
#endif
```

```vb
' Put the following code in your mainform loaded event
' Note that this will not work in the App.xaml.cs Loaded
#If DEBUG Then
    System.Diagnostics.Debug.WriteLine("Windows Store SID = " & Microsoft.Toolkit.Uwp.Services.Facebook.FacebookService.Instance.WindowsStoreId)
#End If
```

> [!NOTE]
> You may have to turn on the Output window in Visual Studio to see this debug writeline.

The above code will output something like this:

```
Windows Store SID = ms-app://s-1-15-2-12341451-1486691014-2395677208-123421631-1234998043-1234490472-123452499/
```

When entering the value into the Facebook Developer site you must strip the ms-app:// and the trailing / off the string.

## Creating a new Application on Facebook Developer Site

1. To get a **Facebook.WindowsStoreID**, go to: <https://developers.facebook.com/apps>.
2. Select **Create a New App ID**, to start integration Facebook into your app or website.
3. Click, **Create a New App**
4. From the app Dashboard choose the **Settings** item on the left.  It should select the *Basic* item under it by default.
5. **+Add Platform** choose Windows App.  Leave the *Namespace* and *App Domains* entries blank.
6. Enter the **Windows Store SID** from within your app (see *Getting Windows Store SID* section)
7. From left side menu choose **+Add Product** Click to add *Facebook Login*.  Ensure you set the following options in the UI:

| Setting | Value |
|----------|------:|
| Client OAuth Login | Yes |
| Web OAuth Login | No |
| Embedded Browser OAuth Login | Yes |
| Force Web OAuth Redirection | No |
| Login from Devices | No |
| Valid OAuth redirect URIs | Blank |

## Syntax

```csharp
// Initialize service
FacebookService.Instance.Initialize(AppIDText.Text);

// Login to Facebook
if (!await FacebookService.Instance.LoginAsync())
{
    return;
}

// Get user's feed
ListView.ItemsSource = await FacebookService.Instance.RequestAsync(FacebookDataConfig.MyFeed, 50);

// Get current user profile picture
ProfileImage.DataContext = await FacebookService.Instance.GetUserPictureInfoAsync();

// Post a message on your wall using Facebook Dialog
await FacebookService.Instance.PostToFeedWithDialogAsync(TitleText.Text, DescriptionText.Text, UrlText.Text);

// Get current user's photo albums
await FacebookService.Instance.GetUserAlbumsAsync();

// Get current user's photos by album Id
await FacebookService.Instance.GetUserPhotosByAlbumIdAsync(addedItem.Id);
```

```vb
' Initialize service
FacebookService.Instance.Initialize(AppIDText.Text)

' Login to Facebook
If Not Await FacebookService.Instance.LoginAsync() Then
    Return
End If

' Get user's feed
ListView.ItemsSource = Await FacebookService.Instance.RequestAsync(FacebookDataConfig.MyFeed, 50)

' Get current user profile picture
ProfileImage.DataContext = Await FacebookService.Instance.GetUserPictureInfoAsync()

' Post a message on your wall using Facebook Dialog
Await FacebookService.Instance.PostToFeedWithDialogAsync(TitleText.Text, DescriptionText.Text, UrlText.Text)

' Get current user's photo albums
Await FacebookService.Instance.GetUserAlbumsAsync()

' Get current user's photos by album Id
Await FacebookService.Instance.GetUserPhotosByAlbumIdAsync(addedItem.Id)
```
