---
title: Wysyłanie powiadomień wypychanych do systemu iOS przy użyciu usługi Azure Notification Hubs i zestawu iOS SDK Version 3.0.0 Preview 1
description: W tym samouczku dowiesz się, jak wysyłać powiadomienia wypychane do urządzeń z systemem iOS przy użyciu usług Azure Notification Hubs i Apple Push Notification Service (wersja 3.0.0-zestawu).
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: 25f18eb0f55560b7abd250b8511b2e250ea55852
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/25/2020
ms.locfileid: "96001341"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-version-300-preview1"></a>Samouczek: wysyłanie powiadomień wypychanych do aplikacji systemu iOS przy użyciu usługi Azure Notification Hubs (wersja 3.0.0-zestawu)

W tym samouczku pokazano, jak za pomocą usługi Azure Notification Hubs wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs SDK w wersji 2.0.4.

Ten samouczek obejmuje następujące kroki:

- Utwórz przykładową aplikację dla systemu iOS.
- Połącz aplikację systemu iOS z usługą Azure Notification Hubs.
- Wysyłanie testowych powiadomień wypychanych.
- Sprawdź, czy aplikacja otrzymuje powiadomienia.

Możesz pobrać kompletny kod dla tego samouczka [z usługi GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples).

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka potrzebne są następujące wymagania wstępne:

- Na komputerze Mac z systemem [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532), wraz z prawidłowym certyfikatem dewelopera zainstalowanym w łańcuchu kluczy.
- IPhone lub iPad z systemem iOS w wersji 10 lub nowszej.
- Urządzenie fizyczne zarejestrowane w [portalu firmy Apple](https://developer.apple.com/)i skojarzone z Twoim certyfikatem.

Przed kontynuowaniem przejdź do poprzedniego samouczka na temat rozpoczynania pracy z [usługą Azure Notification Hubs dla aplikacji systemu iOS](ios-sdk-get-started.md), aby skonfigurować i skonfigurować poświadczenia wypychania w centrum powiadomień. Nawet jeśli nie masz doświadczenia w tworzeniu systemu iOS, należy wykonać następujące czynności.

> [!NOTE]
> Ze względu na wymagania dotyczące konfiguracji powiadomień wypychanych należy wdrożyć i przetestować powiadomienia wypychane na fizycznym urządzeniu z systemem iOS (telefonie iPhone lub tablecie iPad), a nie emulatorze systemu iOS.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Łączenie aplikacji dla systemu iOS z usługą Notification Hubs

1. W środowisku Xcode utwórz nowy projekt dla systemu iOS i wybierz szablon **Single View Application** (Aplikacja z jednym widokiem).

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="Wybierz szablon":::

2. Podczas określania opcji dla nowego projektu upewnij się, że użyto tych samych wartości **Product Name** (Nazwa produktu) i **Organization Identifier** (Identyfikator organizacji), które zostały użyte podczas ustawiania identyfikatora pakietu w portalu dla deweloperów firmy Apple.

3. W obszarze Projekt nawigatora wybierz nazwę projektu w obszarze **obiekty docelowe**, a następnie wybierz kartę **możliwości podpisywania &** . Upewnij się, że wybrano odpowiedni **zespół** dla konta dewelopera firmy Apple. Środowisko Xcode powinno automatycznie ściągnąć utworzony wcześniej profil aprowizowania na podstawie identyfikatora pakietu.

   Jeśli nowy profil aprowizowania utworzony w programie Xcode nie jest widoczny, odśwież profile dla Twojej tożsamości podpisywania. Kliknij pozycję **Xcode** na pasku menu, kliknij pozycję **Preferences** (Preferencje), kliknij kartę **Account** (Konto), kliknij przycisk **View Details** (Pokaż szczegóły), kliknij tożsamość podpisywania, a następnie kliknij przycisk odświeżania w prawym dolnym rogu.

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="Wyświetl szczegóły":::

4. Na karcie **możliwości & podpisywania** wybierz pozycję **+ możliwość**. Kliknij dwukrotnie pozycję **powiadomienia wypychane** , aby ją włączyć.

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="Funkcja":::

5. Dodaj moduły zestawu Azure Notification Hubs SDK.

   Zestaw SDK platformy Azure Notification Hubs można zintegrować z aplikacją za pomocą [Cocoapods](https://cocoapods.org/) lub ręcznie dodając pliki binarne do projektu.

   - Integracja za pośrednictwem Cocoapods: Dodaj następujące zależności do plik podfile, aby uwzględnić w aplikacji usługę Azure Notification Hubs SDK:

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - Uruchom instalację pod, aby zainstalować nowo zdefiniowany pod i otworzyć xcworkspace.

         Jeśli zobaczysz błąd, na przykład **nie można znaleźć specyfikacji dla AzureNotificationHubs-iOS** podczas instalacji pod instalacją, uruchom polecenie, `pod repo update` Aby uzyskać najnowsze Zasobniki z repozytorium Cocoapods, a następnie uruchom instalację pod kontrolą.

   - Integracja przez kopiowanie plików binarnych do projektu:

      Można przeprowadzić integrację, kopiując pliki binarne do projektu w następujący sposób:

        - Pobierz platformę [zestawu SDK usługi Azure Notification Hubs](https://github.com/Azure/azure-notificationhubs-iOS/releases/) w postaci pliku zip i rozpakuj ją.

        - W programie Xcode kliknij prawym przyciskiem myszy projekt i kliknij opcję **Add Files to** (Dodaj pliki do), aby dodać folder **WindowsAzureMessaging.framework** do projektu Xcode. Wybierz pozycję **Options** (Opcje), upewnij się, że pozycja **Copy items if needed** (Skopiuj elementy w razie potrzeby) jest zaznaczona, a następnie kliknij pozycję **Add** (Dodaj).

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="Dodaj strukturę":::

6. Dodaj nowy plik nagłówkowy do projektu o nazwie **stałychs. h**. Aby to zrobić, kliknij prawym przyciskiem myszy nazwę projektu i wybierz pozycję **nowy plik.**... Następnie wybierz pozycję **plik nagłówka**. Ten plik zawiera stałe używane przez centrum powiadomień. Następnie wybierz przycisk **Dalej**. Nazwij plik **stałe. h**.

7. Dodaj następujący kod do pliku stałych. h:

   ```objc
   #ifndef Constants_h
   #define Constants_h
   extern NSString* const NHInfoConnectionString;
   extern NSString* const NHInfoHubName;
   extern NSString* const NHUserDefaultTags;
   #endif /* Constants_h */
   ```

8. Dodaj plik implementacji dla stałych. h. Aby to zrobić, kliknij prawym przyciskiem myszy nazwę projektu i wybierz pozycję **nowy plik.**... Wybierz pozycję **plik zamierzenia C**, a następnie wybierz przycisk **dalej**. Nazwij stałe plik **. m**.

   :::image type="content" source="media/ios-sdk/image5.png" alt-text="Dodaj plik implementacji":::

9. Otwórz plik **stałe. m** i Zastąp jego zawartość następującym kodem. Zastąp symbole zastępcze literałów ciągów `NotificationHubConnectionString` i `NotificationHubConnectionString` odpowiednio nazwą centrum i **DefaultListenSharedAccessSignature**, jak poprzednio uzyskano z portalu:

   ```objc
   #import <Foundation/Foundation.h>
   #import "Constants.h"

   NSString* const NHInfoConnectionString = @"NotificationHubConnectionString";
   NSString* const NHInfoHubName = @"NotificationHubName";NSString* const NHUserDefaultTags = @"notification_tags";
   ```

10. W pliku **AppDelegate. h** projektu Dodaj następującą `import` instrukcję:

    ```objc
    #import "Constants.h"
    ```

11. W tym samym pliku **AppDelegate. m** Zastąp cały kod następującym kodem `didFinishLaunchingWithOptions` :

    ```objc
    // Tells the delegate that the app successfully registered with Apple Push Notification service (APNs).


    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    NSString *hubName = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoHubName];
    NSString *connectionString = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoConnectionString];
    [MSNotificationHub initWithConnectionString:connectionString withHubName:hubName];


    NSMutableSet *tags = [[NSMutableSet alloc] init];

    // Load and parse stored tags
    NSString *unparsedTags = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
    if (unparsedTags.length > 0) {
        NSArray *tagsArray = [unparsedTags componentsSeparatedByString: @","];

        [MSNotificationHub addTags:tagsArray];
    }

    }
    - (void)showAlert:(NSString *)message withTitle:(NSString *)title {
    if (title == nil) {
        title = @"Alert";
    }
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
    [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alert animated:YES completion:nil];
    }

    - (void)showNotification:(NSDictionary *)userInfo {
    [self logNotificationDetails:userInfo];

    NotificationDetailViewController *notificationDetail = [[NotificationDetailViewController alloc] initWithUserInfo:userInfo];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:notificationDetail animated:YES completion:nil];
    }

    @end
    ```

    Ten kod nawiązuje połączenie z centrum powiadomień przy użyciu informacji o połączeniu określonych w polu **stałe. h**. Następnie przekazuje token urządzenia do centrum powiadomień, aby umożliwić wysyłanie powiadomień przez centrum.

### <a name="create-notificationdetailviewcontroller-header-file"></a>Utwórz plik nagłówka NotificationDetailViewController

1. Podobnie jak w poprzednich instrukcjach, Dodaj kolejny plik nagłówka o nazwie **NotificationDetailViewController. h**. Zastąp zawartość nowego pliku nagłówka następującym kodem:

   ```objc
   #import <UIKit/UIKit.h>

   NS_ASSUME_NONNULL_BEGIN

   @interface NotificationDetailViewController : UIViewController

   @property (strong, nonatomic) IBOutlet UILabel *titleLabel;
   @property (strong, nonatomic) IBOutlet UILabel *bodyLabel;
   @property (strong, nonatomic) IBOutlet UIButton *dismissButton;

   @property (strong, nonatomic) NSDictionary *userInfo;

   - (id)initWithUserInfo:(NSDictionary *)userInfo;

   @end

   NS_ASSUME_NONNULL_END
   ```

2. Dodaj plik implementacji **NotificationDetailViewController. m**. Zastąp zawartość pliku następującym kodem, który implementuje metody UIViewController:

   ```objc
   #import "NotificationDetailViewController.h"

   @interface NotificationDetailViewController ()

   @end

   @implementation NotificationDetailViewController

   - (id)initWithUserInfo:(NSDictionary *)userInfo {
    self = [super initWithNibName:@"NotificationDetail" bundle:nil];
    if (self) {
        _userInfo = userInfo;
    }
    return self;
   }

   - (void)viewDidLayoutSubviews {
    [self.titleLabel sizeToFit];
    [self.bodyLabel sizeToFit];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    NSString *title = nil;
    NSString *body = nil;

    NSDictionary *aps = [_userInfo valueForKey:@"aps"];
    NSObject *alertObject = [aps valueForKey:@"alert"];
    if (alertObject != nil) {
        if ([alertObject isKindOfClass:[NSDictionary class]]) {
            NSDictionary *alertDict = (NSDictionary *)alertObject;
            title = [alertDict valueForKey:@"title"];
            body = [alertObject valueForKey:@"body"];
        } else if ([alertObject isKindOfClass:[NSString class]]) {
            body = (NSString *)alertObject;
        } else {
            NSLog(@"Unable to parse notification content. Unexpected format: %@", alertObject);
        }
    }

    if (title == nil) {
        title = @"<unset>";
    }

    if (body == nil) {
        body = @"<unset>";
    }

    self.titleLabel.text = title;
    self.bodyLabel.text = body;
   }

   - (IBAction)handleDismiss:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
   }

   @end
   ```

### <a name="viewcontroller"></a>Plik viewcontroller

1. W pliku Project **plik viewcontroller. h** Dodaj następujące `import` instrukcje:

   ```objc
   #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
   #import <UserNotifications/UserNotifications.h>
   ```

2. Również w **plik viewcontroller. h** Dodaj następującą deklarację właściwości po `@interface` deklaracji:

   ```objc
   @property (strong, nonatomic) IBOutlet UITextField *tagsTextField;
   ```

3. W pliku implementacji **plik viewcontroller. m** projektu Zastąp zawartość pliku następującym kodem:

   ```objc
   #import "ViewController.h"
   #import "Constants.h"
   #import "AppDelegate.h"

   @interface ViewController ()

   @end

   @implementation ViewController

   // UIViewController methods

   - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    // Simple method to dismiss keyboard when user taps outside of the UITextField.
    [self.view endEditing:YES];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    // Load raw tags text from storage and initialize the text field
    self.tagsTextField.text = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
   }

   - (IBAction)handleRegister:(id)sender {
    // Save raw tags text in storage
    [[NSUserDefaults standardUserDefaults] setValue:self.tagsTextField.text forKey:NHUserDefaultTags];

   // Delegate processing the register action to the app delegate.
   [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleRegister)];
   }

   - (IBAction)handleUnregister:(id)sender {
    [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleUnregister)];
   }

   @end
   ```

4. Skompiluj i uruchom aplikację na urządzeniu, aby upewnić się, że nie występują żadne błędy.

## <a name="send-test-push-notifications"></a>Wysyłanie testowych powiadomień wypychanych

Odbieranie powiadomień w aplikacji możesz przetestować za pomocą opcji **Wysyłanie testowe** w witrynie [Azure Portal](https://portal.azure.com/). Powoduje to wysłanie testowego powiadomienia push na urządzenie.

:::image type="content" source="media/ios-sdk/image6.png" alt-text="Wysyłanie testowe":::

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi wewnętrznej bazy danych, takiej jak Mobile Apps czy ASP.NET, przy użyciu zgodnej biblioteki. Jeśli biblioteka nie jest dostępna dla zaplecza, można także użyć interfejsu API REST bezpośrednio do wysyłania komunikatów powiadomień.

Poniżej znajduje się lista innych samouczków, które warto przejrzeć w celu wysłania powiadomień:

- Azure Mobile Apps: Aby zapoznać się z przykładem wysyłania powiadomień z Mobile Apps zaplecza zintegrowanego z usługą Notification Hubs, zobacz [Dodawanie powiadomień wypychanych do aplikacji systemu iOS](/previous-versions/azure/app-service-mobile/app-service-mobile-ios-get-started-push).
- ASP.NET: [użyj Notification Hubs, aby wysyłać powiadomienia wypychane do użytkowników](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).
- Zestaw SDK Java usługi Azure Notification Hubs: zobacz [Jak używać usługi Notification Hubs za pomocą języka Java](notification-hubs-java-push-notification-tutorial.md), aby zapoznać się ze sposobem wysyłania powiadomień za pomocą języka Java. To rozwiązanie przetestowano w programie Eclipse pod kątem tworzenia aplikacji dla systemu Android.
- PHP: [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md) (Używanie usługi Notification Hubs z poziomu języka PHP).

## <a name="verify-that-your-app-receives-push-notifications"></a>Sprawdzanie, czy aplikacja odbiera powiadomienia push

Aby przetestować powiadomienia wypychane w systemie iOS, należy wdrożyć aplikację na fizycznym urządzeniu z systemem iOS. Powiadomień wypychanych firmy Apple nie można wysyłać za pomocą symulatora systemu iOS.

1. Uruchom aplikację i upewnij się, że rejestracja zakończyła się powodzeniem, a następnie naciśnij przycisk **OK**.

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="Zarejestruj":::

2. Następnie Wyślij testowe Powiadomienie wypychane z [Azure Portal](https://portal.azure.com/), zgodnie z opisem w poprzedniej sekcji.

3. Powiadomienie wypychane jest wysyłane do wszystkich urządzeń zarejestrowanych w celu otrzymywania powiadomień z danego centrum powiadomień.

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="Wyślij test":::

## <a name="next-steps"></a>Następne kroki

W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich zarejestrowanych urządzeń z systemem iOS. Aby dowiedzieć się, jak wysyłać powiadomienia wypychane do określonych urządzeń z systemem iOS, przejdź do następującego samouczka:

[Samouczek: powiadomienia wypychane do określonych urządzeń](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Omówienie usługi Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [Interfejsy API REST Notification Hubs](/rest/api/notificationhubs/)
- [Notification Hubs SDK dla operacji zaplecza](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification Hubs SDK w witrynie GitHub](https://github.com/Azure/azure-notificationhubs)
- [Rejestrowanie przy użyciu zaplecza aplikacji](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Zarządzanie rejestracją](notification-hubs-push-notification-registration-management.md)
- [Praca ze znacznikami](notification-hubs-tags-segment-push-message.md)
- [Praca z szablonami niestandardowymi](notification-hubs-templates-cross-platform-push-messages.md)
- [Service Bus kontroli dostępu z sygnaturami dostępu współdzielonego](../service-bus-messaging/service-bus-sas.md)
- [Programowe generowanie tokenów SAS](/rest/api/eventhub/generate-sas-token)
- [Zabezpieczenia firmy Apple: typowe Kryptografia](https://developer.apple.com/security/)
- [Czas epoki systemu UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [FUNKCJE](https://en.wikipedia.org/wiki/HMAC)