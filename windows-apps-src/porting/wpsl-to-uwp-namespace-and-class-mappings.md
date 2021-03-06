---
description: Cette rubrique présente le mappage complet des API de Silverlight pour Windows Phone sur leurs équivalents de la plateforme Windows universelle (UWP).
title: WPSL à l’espace de noms UWP et aux mappages de classe
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ab6bdace41041f03bd4316b1f65de1a5aeb60835
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167493"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone des mappages d’API Silverlight vers UWP


Cette rubrique présente le mappage complet des API de Silverlight pour Windows Phone sur leurs équivalents de la plateforme Windows universelle (UWP). Il ne s’agit généralement pas d’un mappage un-à-un des fonctionnalités : une plateforme peut comporter plus ou moins de fonctionnalités que l’autre dans un espace de noms ou dans une classe.

La table de mappage vous aidera si vous travaillez dans un projet UWP et que vous réutilisez le code source d’un projet Silverlight pour Windows Phone. Il existe des différences dans les noms des espaces de noms et des classes (y compris dans les contrôles d’interface utilisateur) entre les deux plateformes. Dans de nombreux cas, pour effectuer un mappage, il suffit de modifier un nom d’espace de noms. Votre code est ensuite compilé. Parfois, un nom de classe ou d’API a changé, ainsi que le nom de l’espace de noms. D’autres fois, le mappage demande un peu plus de travail, et dans de rares cas, il nécessite un changement d’approche.

**Comment utiliser la table :  ** Commencez par Rechercher le nom de la classe que vous utilisez. Les classes sont indiquées lorsque le mappage est plus complexe qu’un simple changement de nom de l’espace de noms. Si votre classe n’est pas répertoriée, le mappage correspond simplement à une modification de l’espace de noms. Recherchez le nom de l’espace de noms de votre classe pour trouver son équivalent UWP. Votre classe figurera dans cet espace de noms. Si votre espace de noms n’est pas indiqué, son nom n’a pas changé.

**Remarque**    Windows 10 prend en charge bien plus d' .NET Framework qu’une application Windows Phone Store. Par exemple, Windows 10 possède plusieurs System. ServiceModel. \* les espaces de noms, ainsi que System.Net, System .net. NetworkInformation et System .net. Sockets.
En outre, dans une application Windows 10, vous bénéficierez du code natif .NET, dont une technologie de compilation d’avant-garde qui convertit le MSIL en code machine exécutable en mode natif. Les applications .NET Native démarrent plus vite, utilisent moins de mémoire et consomment moins de batterie que leurs équivalents MSIL.

| Windows Phone Silverlight | Windows Runtime |
| ------------------------- | --------------- |
| Publicités | |
| Classe **Microsoft.Advertising.Mobile.UI.AdControl** | Classe [AdControl](../monetize/display-ads-in-your-app.md) |
| Alarmes, rappels et agents d’arrière-plan | |
| Classe **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) , classe |
| Espace de noms **Microsoft.Phone.Scheduler** | Espace de noms [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background) |
| Classe **Microsoft.Phone.Scheduler.Alarm** | Classes [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) et [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| Classes **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask** et **ScheduledTaskAgent** | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) , classe |
| Classe **Microsoft.Phone.Scheduler.Reminder** | Classes [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) et [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| Classe **Microsoft.Phone.PictureDecoder** | Classe [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) |
| Espace de noms **Microsoft.Phone.BackgroundAudio** | Espace de noms [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback) |
| Espace de noms **Microsoft.Phone.BackgroundTransfer** | Espace de noms [**Windows.Networking.BackgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) |
| Environnement et modèle d’application | |
| Classe **System.AppDomain** | Aucun équivalent direct. Voir les classes [**Application**](/uwp/api/Windows.UI.Xaml.Application) et [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication). |
| Classe **System.Environment** | Aucun équivalent direct |
| Classe **System.ComponentModel.Annotations**  | Aucun équivalent direct |
| Classe **System.ComponentModel.BackgroundWorker** | Classe [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) |
| Classe **System.ComponentModel.DesignerProperties** | Classe [**DesignMode**](/uwp/api/Windows.ApplicationModel.DesignMode) |
| Classes **System.Threading.Thread** et **System.Threading.ThreadPool** | Classe [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) |
| (ST = **System.Threading**) <br/> Méthode **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Méthode **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propriété **S.Environment.ManagedThreadId** |
| Classe **System.Threading.Timer** | [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) , classe |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.Dispatcher** | [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) , classe |
| (SWT = **System.Windows.Threading**) <br/> Classe **SWT.DispatcherTimer** | Classe [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) |
| Blend pour Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Classe **MEDC.GeometryHelper** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity** | Espace de noms [Microsoft.Xaml.Interactivity](/previous-versions/dn458193(v=vs.120)) |
| Espace de noms **Microsoft.Expression.Interactivity.Core** | Espace de noms [Microsoft.Xaml.Interactions.Core](/previous-versions/dn458182(v=vs.120)) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Classe **MEIC.ExtendedVisualStateManager** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity.Input** | Aucun équivalent direct |
| Espace de noms **Microsoft.Expression.Interactivity.Media** | Espace de noms [Microsoft.Xaml.Interactions.Media](/previous-versions/dn458186(v=vs.120)) |
| Espace de noms **Microsoft.Expression.Shapes** | Aucun équivalent direct |
| (MI = **Microsoft.Internal**) <br/> Interface **MI.IManagedFrameworkInternalHelper** | Aucun équivalent direct |
| Données de contacts et calendrier | |
| Espace de noms **Microsoft.Phone.UserData** | Espaces de noms [**Windows. ApplicationModel. contacts**](/uwp/api/Windows.ApplicationModel.Contacts), [**Windows. ApplicationModel. rendez-vous**](/uwp/api/Windows.ApplicationModel.Appointments) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classes **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress** et **ContactPhoneNumber** | Classe [**contact**](/uwp/api/Windows.ApplicationModel.Contacts.Contact) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Appointments** | Classe [**AppointmentCalendar**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Classe **MPU.Contacts** | [**ContactStore**](/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) , classe |
| Contrôles et infrastructure d’interface utilisateur | |
| Classe **ControlTiltEffect.TiltEffect** | Des animations de la bibliothèque d’animations Windows Runtime sont intégrées aux styles par défaut des contrôles courants. Voir [Animation](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **Microsoft.Phone.Controls** | Espace de noms [**Windows. UI. Xaml. Controls**](/uwp/api/Windows.UI.Xaml.Controls) |
| (MPC = **Microsoft.Phone.Controls**) <br/> Classe **MPC.ContextMenu** | [**PopupMenu**](/uwp/api/Windows.UI.Popups.PopupMenu) (classe) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.DatePickerPage** | [**DatePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) , classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.GestureListener** | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) , classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.LongListSelector** | [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) , classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.ObscuredEventArgs** | [**SystemProtection**](/uwp/api/Windows.Phone.System.SystemProtection), classes [**WindowActivatedEventArgs**](/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.Panorama** | Classe de [**concentrateur**](/uwp/api/Windows.UI.Xaml.Controls.Hub) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classes **MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** | Classe [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.PhoneApplicationPage** | Classe de [**page**](/uwp/api/Windows.UI.Xaml.Controls.Page) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TiltEffect** | [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) , classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.TimePickerPage** | [**TimePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) , classe |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowser** | [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) (classe) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WebBrowserExtensions** | Aucun équivalent direct |
| (MPC = **Microsoft.Phone.Controls**) <br/>Classe **MPC.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) et [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) peuvent être utilisés dans le modèle du panneau éléments d’un contrôle d’éléments. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD.Linq** | Aucun équivalent direct |
| (MPD = **Microsoft.Phone.Data**) <br/>Espace de noms **MPD. Linq.Mapping** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Globalization** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classes **MPI.DeviceExtendedProperties** et **DeviceStatus** | [**EasClientDeviceInformation**](/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [**memorymanager**](/uwp/api/Windows.System.MemoryManager) , classes. Pour plus d’informations, voir [État de l’appareil](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.MediaCapabilities** | Aucun équivalent direct |
| (MPI = **Microsoft.Phone.Info**) <br/>Classe **MPI.UserExtendedProperties** | [**AdvertisingManager**](/uwp/api/Windows.System.UserProfile.AdvertisingManager) , classe |
| Espace de noms **System.Windows** | Espace de noms [**Windows.UI.Xaml**](/uwp/api/Windows.UI.Xaml) |
| Espace de noms **System.Windows.Automation** | Espace de noms [**Windows. UI. Xaml. Automation**](/uwp/api/Windows.UI.Xaml.Automation) |
| Espaces de noms **System.Windows.Controls** et **System.Windows.Input** | Espaces de noms [**Windows. UI. Core**](/uwp/api/Windows.UI.Core), [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Windows. UI. Xaml. Controls**](/uwp/api/Windows.UI.Xaml.Controls) |
| Classes **System.Windows.Controls.DrawingSurface** et **DrawingSurfaceBackgroundGrid** | Classe [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) |
| Classe **System.Windows.Controls.RichTextBox** | [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) , classe |
| Classe **System.Windows.Controls.WrapPanel** | Aucun équivalent direct à des fins de disposition générale. [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) et [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) peuvent être utilisés dans le modèle du panneau éléments d’un contrôle d’éléments. |
| Espace de noms **System.Windows.Controls.Primitives** | Espace de noms [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) |
| Espace de noms **System.Windows.Controls.Shapes** | Espace de noms [**Windows. UI. Xaml. Controls. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Espace de noms **System.Windows.Data** | Espace de noms [**Windows. UI. Xaml. Data**](/uwp/api/Windows.UI.Xaml.Data) |
| Espace de noms **System.Windows.Documents** | Espace de noms [**Windows.UI.Xaml.Documents**](/uwp/api/Windows.UI.Xaml.Documents) |
| Espace de noms **System.Windows.Ink** | Aucun équivalent direct |
| Espace de noms **System.Windows.Markup** | Espace de noms [**Windows. UI. Xaml. Markup**](/uwp/api/Windows.UI.Xaml.Markup) | 
| Espace de noms **System.Windows.Navigation** | Espace de noms [**Windows. UI. Xaml. navigation**](/uwp/api/Windows.UI.Xaml.Navigation) |
| Événement **System.Windows.UIElement.Tap**, délégué **EventHandler&lt;GestureEventArgs&gt;** | Événement [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped), délégué [**TappedEventHandler**](/uwp/api/windows.ui.xaml.input.tappedeventhandler) |
| Données et services |  |
| Classe **System.Data.Linq.DataContext** | Aucun équivalent direct |
| Classe **System.Data.Linq.Mapping.ColumnAttribute** | Aucun équivalent direct |
| Classe **System.Data.Linq.SqlClient.SqlHelpers** | Aucun équivalent direct |
| Appareils | |
| Espaces de noms **Microsoft.Devices** et **Microsoft.Devices.Sensors** | Espaces de noms [**Windows. Devices. Enumeration**](/uwp/api/Windows.Devices.Enumeration), [**Windows. Devices. Enumeration. PNP**](/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows. appareils. Input**](/uwp/api/Windows.Devices.Input), [**Windows. Devices. Sensors**](/uwp/api/Windows.Devices.Sensors) |
| Classes **Microsoft.Devices.Camera** et **Microsoft.Devices.PhotoCamera** | Classe [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . Également la classe [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) (Windows uniquement). |
| Classe **Microsoft.Devices.CameraButtons** | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) , classe |
| Classe **Microsoft.Devices.CameraVideoBrushExtensions** | Classe [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) |
| Classe **Microsoft.Devices.Environment** | Aucun équivalent direct. Pour contourner ce problème, utilisez la compilation conditionnelle et définissez un symbole personnalisé. Vous pouvez peut-être concevoir une solution de contournement à l’aide de la propriété [IsAttached](/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached). |
| Classe **Microsoft.Devices.MediaHistory** | Aucun équivalent direct |
| Classe **Microsoft.Devices.VibrateController** | [**VibrationDevice**](/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) , classe |
| Classe **Microsoft.Devices.Radio.FMRadio** | Aucun équivalent direct |
| Classes **Microsoft.Devices.Sensors.Accelerometer** et **Compass** | Dans l’espace de noms [**Windows. Devices. Sensors**](/uwp/api/Windows.Devices.Sensors) |
| Classe **Microsoft.Devices.Sensors.Gyroscope** | Classe [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) |
| Classe **Microsoft.Devices.Sensors.Motion** | [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) , classe |
| Globalisation | |
| Espace de noms **System. Globalization** | Espace de noms [**Windows. Globalization**](/uwp/api/Windows.Globalization) |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propriété **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propriété **S.CultureInfo.CurrentUICulture** |
| Graphisme et animation | |
| **Microsoft. XNA. Framework. \* ** Namespaces, [bibliothèque de classes XNA Framework](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)), [bibliothèque de classes de pipeline de contenu](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | Aucun équivalent direct. En règle générale, utilisez [Microsoft DirectX](/windows/desktop/directx) avec C++. Voir [Développement de jeux](/previous-versions/windows/apps/hh452744(v=win.10)) et [Interopérabilité de DirectX et XAML](/previous-versions/windows/apps/hh825871(v=win.10)). |
| Classe **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) , classe |
| Classe **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) (classe) |
| Espace de noms **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS. Espace de noms UserProfile. GameServices. Core**](/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) |
| Classe **Microsoft.Xna.Framework.GamerServices.Guide** | Aucun équivalent direct |
| Classe **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) , classe |
| Classe **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) , classe |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Classes **MXFM.MediaLibrary** et **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**Fichier KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) , classe |
| Classe **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) , classe |
| Classe **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer**](/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) , classe |
| Espace de noms **System.Windows.Media** | Espace de noms [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) |
| Classe **System.Windows.Media.RadialGradientBrush** | Aucun équivalent direct. Voir [Média et graphismes](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espace de noms **System.Windows.Media.Animation** | Espace de noms [**Windows. UI. Xaml. Media. Animation**](/uwp/api/Windows.UI.Xaml.Media.Animation) |
| Espace de noms **System.Windows.Media.Effects** | Aucun équivalent direct |
| Espace de noms **System.Windows.Media.Imaging** | Espace de noms [**Windows. UI. Xaml. Media. Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) |
| Espace de noms **System.Windows.Media.Media3D** | Espace de noms [**Windows. UI. Xaml. Media. Media3D**](/uwp/api/Windows.UI.Xaml.Media.Media3D) |
| Espace de noms **System.Windows.Shapes** | Espace de noms [**Windows. UI. Xaml. Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Lanceurs et sélecteurs | |
| Classes **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask** et **PhoneNumberChooserTask** | [**ContactPicker**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) , classe |
| Classes **Microsoft.Phone.Tasks.AddWalletItemTask** et **AddWalletItemResult** | Espace de noms [**Windows. ApplicationModel. Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) |
| Classes **Microsoft.Phone.Tasks.BingMapsDirectionsTask** et **BingMapsTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.CameraCaptureTask** | Classe [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . Également la classe [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) (Windows uniquement). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) , classe (méthode[**RequestAppPurchaseAsync**](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) ) |
| Classes **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask** et **WebBrowserTask** | [**Launcher**](/uwp/api/Windows.System.Launcher) (classe) |
| Classe **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) , classe |
| Classe **Microsoft.Phone.Tasks.GameInviteTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask** et **MapUpdaterTask** | Aucun équivalent direct |
| Classe **Microsoft.Phone.Tasks.PhoneCallTask** | Classe [**PhoneCallManager**](/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) |
| Classe **Microsoft.Phone.Tasks.PhotoChooserTask** | Classe [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) |
| Classe **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) , classe |
| Classes **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask** et **SavePhoneNumberTask** | Classe [**StoredContact**](/uwp/api/Windows.Phone.PersonalInformation.StoredContact) (Windows Phone uniquement) | 
| Classe **Microsoft.Phone.Tasks.SaveRingtoneTask** | Aucun équivalent direct |
| Classes **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask** et **ShareStatusTask** | [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) , classe |
| Emplacement | |
| Espace de noms **System.Device.Location** | Espace de noms [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) |
| Classe **System.Device.GeoCoordinateWatcher** | Classe de [**géolocalisation**](/uwp/api/Windows.Devices.Geolocation.Geolocator) |
| Maps | |
| Espaces de noms **Microsoft.Phone.Maps** | Espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) |
| Espace de noms **Microsoft.Phone.Maps.Controls** | Espace de noms [**Windows. UI. Xaml. Controls. Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) |
| Classe **Microsoft.Phone.Maps.Controls.Map** | [**Collection MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) , classe |
| Espace de noms **Microsoft.Phone.Maps.Services** | Espace de noms [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) |
| Classes **Microsoft.Phone.Maps.Services.GeocodeQuery** et **ReverseGeocodeQuery** | [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) , classe |
| Classe **System.Device.Location.GeoCoordinate** | Classe [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) |
| Classe **Microsoft.Phone.Maps.Services.Route** | [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) , classe |
| Classe **Microsoft.Phone.Maps.Services.RouteQuery** | Classe [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) |
| Monétisation | |
| Espace de noms **Microsoft.Phone.Marketplace** | Espace de noms [**Windows. ApplicationModel. Store**](/uwp/api/Windows.ApplicationModel.Store) |
| Multimédia | |
| Espace de noms **Microsoft.Phone.Media** | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) (classe) |
| Mise en réseau | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.DeviceNetworkInformation** | [**Hostname**](/uwp/api/Windows.Networking.HostName), classes [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterface** | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) (classe) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile**](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) , classe |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.NetworkInterfaceList** | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) (classe) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.SocketExtensions** | Aucun équivalent direct |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Classe **MPNN.WebRequestExtensions** | Aucun équivalent direct |
| Espace de noms **Microsoft.Phone.Networking.Voip** | Aucun équivalent direct |
| Classe **System.Net.CookieCollection** | Toujours pris en charge, mais certaines propriétés sont manquantes (par exemple, IsReadOnly) |
| Classe **System.Net.DownloadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | Classe [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Dérive de [System.Net.Http.StreamContent](/previous-versions/visualstudio/hh138119(v=vs.118)) pour mesurer la progression. |
| Classes **System.Net.DnsEndPoint** et **IPAddress** | Ces classes sont toujours prises en charge, mais certaines propriétés sont manquantes. Vous pouvez également utiliser le port dans la classe [**hostname**](/uwp/api/Windows.Networking.HostName) . |
| Classe **System.Net.HttpUtility** | [**HtmlFormatHelper**](/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) , classe |
| Classe **System.Net.HttpWebRequest** | Prise en charge partielle, mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) pour représenter une requête HTTP. |
| Classe **System.Net.HttpWebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) pour représenter une réponse HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Classe **SNN.NetworkChange** | Toujours pris en charge, à l’exception du constructeur. |
| Classe **System.Net.OpenReadCompletedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) , classe (ou [System .net. http. httpclient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.Sockets.Socket** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Vous pouvez également utiliser le port dans la classe[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) . |
| Classe **System.Net.Sockets.SocketException** | Toujours pris en charge, mais utilisez la propriété SocketErrorCode au lieu d’ErrorCode. |
| Classes **System.Net.Sockets.UdpAnySourceMulticastClient** et **UdpSingleSourceMulticastClient** | [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) , classe |
| Classe **System.Net.UploadProgressChangedEventArgs** et classes similaires associées à **System.Net.WebClient** | [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) , classe (ou [System .net. http. httpclient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebClient** | [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) , classe (ou [System .net. http. httpclient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.Net.WebRequest** | Prise en charge partielle (ensemble de propriétés différent), mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) pour représenter une requête HTTP. |
| Classe **System.Net.WebResponse** | Toujours pris en charge, mais utilisez Dispose() au lieu de Close(). Mais l’alternative prospective recommandée correspond à la classe [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (ou [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Ces API utilisent [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) pour représenter une réponse HTTP. |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventArgs** | [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) , classe (ou [System .net. http. httpclient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> Classe **SN.WriteStreamClosedEventHandler** | [**Httpclient**](/uwp/api/Windows.Web.Http.HttpClient) , classe (ou [System .net. http. httpclient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Classe **System.UriFormatException** | Classe **System.FormatException** |
| Notifications | |
| Espace de noms MPN = **Microsoft.Phone.Notification** | Espaces de noms [**Windows. UI. notifications**](/uwp/api/Windows.UI.Notifications), [**Windows. Networking. PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotification** | Classe [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification) |
| MPN = **Microsoft.Phone.Notification** <br/> Classe **MPN.HttpNotificationChannel** | [**PushNotificationChannel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) , classe |
| Programmation | |
| Espace de noms **System** | Espace de noms [**Windows. Foundation**](/uwp/api/Windows.Foundation) |
| Classes **System.Diagnostics.StackFrame** et **StackTrace** | Aucun équivalent direct |
| Espace de noms **System.Diagnostics** | Espace de noms [**Windows.Foundation.Diagnostics**](/uwp/api/Windows.Foundation.Diagnostics) |
| Interface **System.ICloneable** | Méthode personnalisée renvoyant le type approprié. |
| Classe **System.Reflection.Emit.ILGenerator** | Aucun équivalent direct |
| Extensions réactives | |
| Espace de noms **Microsoft.Phone.Reactive** | Aucun équivalent direct |
| Réflexion | |
| Classe **System.Type** | Classe **System.Reflection.TypeInfo**. Consultez [la réflexion dans le .NET Framework pour les applications UWP](/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Ressources | |
| Classe **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**Wa. Resources. Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) et [**wa. **](/uwp/api/Windows.ApplicationModel.Resources) Espaces de noms de ressources, classe [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) . Voir [Création et récupération de ressources dans les applications Windows Runtime](/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Élément sécurisé | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classes **MPS.SecureElementChannel** et **MPS.SecureElementSession** | [**SmartCardConnection**](/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) , classe |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Classe **MPS.SecureElementReader** | [**SmartCardReader**](/uwp/api/Windows.Devices.SmartCards.SmartCardReader) , classe |
| Sécurité | |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.Aes** et **SSC.RSA** | Classe [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) |
| (SSC = **System.Security.Cryptography**) <br/> Classes **SSC.HMACSHA256** et **SSC.SHA256** | [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) , classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.ProtectedData** | [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) , classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.RandomNumberGenerator** | [**CryptographicBuffer**](/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) , classe |
| (SSC = **System.Security.Cryptography**) <br/> Classe **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager**](/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) , classe |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBar** | [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) (classe) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarIconButton** | Classe [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) (lorsqu’elle est utilisée à l’intérieur de la propriété [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ApplicationBarMenuItem** | Classe [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) (en cas d’utilisation dans la propriété [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) ) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classes **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData** et **MPSh.StandardTileData** | Classe [**TileTemplateType**](/uwp/api/Windows.UI.Notifications.TileTemplateType) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.PhoneApplicationService** | [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication), classes [**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator**](/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) , classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTile** | Classe [**SecondaryTile**](/uwp/api/Windows.UI.StartScreen.SecondaryTile) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellTileSchedule** | [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater) , classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.ShellToast** | [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) , classe |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Classe **MPSh.SystemTray** | [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) (classe) |
| Stockage et E/S | |
| Classes **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile** et **ExternalStorageFolder** | [**Fichier KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) , classe |
| Espace de noms **System.IO** | Espaces de noms [**Windows.Storage**](/uwp/api/Windows.Storage) et [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams) |
| Classe **System.IO.Directory** | [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) , classe |
| Classe **System.IO.File** | Classes [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) et [**PathIO**](/uwp/api/Windows.Storage.PathIO)
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageFile** |[**ApplicationData. LocalFolder,**](/uwp/api/windows.storage.applicationdata.localfolder) propriété |
| (SII = **System.IO.IsolatedStorage**) <br/> Classe **SII.IsolatedStorageSettings** | [**ApplicationData. LocalSettings,**](/uwp/api/windows.storage.applicationdata.localsettings) propriété |
| Classe **System.IO.Stream** | Toujours pris en charge, mais utilisez ReadAsync() et WriteAsync() au lieu de BeginRead()/EndRead() et BeginWrite()/EndWrite(). |
| Wallet | |
| Espace de noms **Microsoft.Phone.Wallet** | Espace de noms [**Windows. ApplicationModel. Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) |
| Xml | |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Méthode **SX.XmlConvert.ToDateTimeOffset** |


Rubrique suivante : [Portage du projet](wpsl-to-uwp-porting-to-a-uwp-project.md).