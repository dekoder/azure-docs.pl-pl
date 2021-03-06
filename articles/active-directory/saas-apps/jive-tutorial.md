---
title: 'Samouczek: integracja Azure Active Directory z usługą Jive | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Logowanie jednokrotne między Azure Active Directory i Jive.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/16/2020
ms.author: jeedes
ms.openlocfilehash: 3d15e5e13b2b0defe45fd45450aee7c262052b4f
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/23/2020
ms.locfileid: "92459431"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jive"></a>Samouczek: Azure Active Directory integracji logowania jednokrotnego (SSO) z usługą Jive

W tym samouczku dowiesz się, jak zintegrować usługę Jive z usługą Azure Active Directory (Azure AD). Po zintegrowaniu usługi Jive z usługą Azure AD można:

* Kontrolka w usłudze Azure AD, która ma dostęp do Jive.
* Zezwól użytkownikom na automatyczne logowanie się do usługi Jive przy użyciu kont w usłudze Azure AD.
* Zarządzaj kontami w jednej centralnej lokalizacji — Azure Portal.

Aby dowiedzieć się więcej o integracji aplikacji SaaS z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne przy użyciu Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz subskrypcji, możesz uzyskać [bezpłatne konto](https://azure.microsoft.com/free/).
* Subskrypcja z włączonym logowaniem jednokrotnym (SSO) Jive.

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i testujesz Logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Jive obsługuje logowanie jednokrotne w usłudze **SP**
* Jive obsługuje [ **Automatyczne** Inicjowanie obsługi użytkowników](jive-provisioning-tutorial.md)
* Po skonfigurowaniu Jive można wymusić kontrolki sesji, które chronią eksfiltracji i niefiltrowanie danych poufnych organizacji w czasie rzeczywistym. Kontrolki sesji wykraczają poza dostęp warunkowy. [Dowiedz się, jak wymuszać kontrolę sesji za pomocą Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad)

## <a name="adding-jive-from-the-gallery"></a>Dodawanie Jive z galerii

Aby skonfigurować integrację programu Jive z usługą Azure AD, musisz dodać Jive z galerii do listy zarządzanych aplikacji SaaS.

1. Zaloguj się do [Azure Portal](https://portal.azure.com) przy użyciu konta służbowego lub konto Microsoft prywatnego.
1. W okienku nawigacji po lewej stronie wybierz usługę **Azure Active Directory** .
1. Przejdź do **aplikacji przedsiębiorstwa** , a następnie wybierz pozycję **wszystkie aplikacje**.
1. Aby dodać nową aplikację, wybierz pozycję **Nowa aplikacja**.
1. W sekcji **Dodaj z galerii** wpisz **Jive** w polu wyszukiwania.
1. Wybierz pozycję **Jive** from panel wyników, a następnie Dodaj aplikację. Poczekaj kilka sekund, gdy aplikacja zostanie dodana do dzierżawy.


## <a name="configure-and-test-azure-ad-single-sign-on-for-jive"></a>Skonfiguruj i przetestuj Logowanie jednokrotne w usłudze Azure AD dla Jive

Skonfiguruj i przetestuj Logowanie jednokrotne usługi Azure AD za pomocą Jive przy użyciu użytkownika testowego o nazwie **B. Simon**. Aby logowanie jednokrotne działało, należy ustanowić relację linku między użytkownikiem usługi Azure AD i powiązanym użytkownikiem w Jive.

Aby skonfigurować i przetestować Logowanie jednokrotne usługi Azure AD za pomocą Jive, wykonaj następujące bloki konstrukcyjne:

1. **[Skonfiguruj Logowanie jednokrotne usługi Azure AD](#configure-azure-ad-sso)** , aby umożliwić użytkownikom korzystanie z tej funkcji.
    * **[Utwórz użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)** — aby przetestować Logowanie jednokrotne w usłudze Azure AD za pomocą usługi B. Simon.
    * **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)** — aby umożliwić usłudze B. Simon korzystanie z logowania jednokrotnego w usłudze Azure AD.
1. **[Skonfiguruj Logowanie jednokrotne](#configure-jive-sso)** w usłudze Jive, aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
    * **[Utwórz użytkownika testowego Jive](#create-jive-test-user)** , aby dysponować odpowiednikiem B. Simon w Jive, która jest połączona z reprezentacją użytkownika w usłudze Azure AD.
1. **[Przetestuj Logowanie jednokrotne](#test-sso)** — aby sprawdzić, czy konfiguracja działa.

## <a name="configure-azure-ad-sso"></a>Konfigurowanie rejestracji jednokrotnej w usłudze Azure AD

Wykonaj następujące kroki, aby włączyć logowanie jednokrotne usługi Azure AD w Azure Portal.

1. W [Azure Portal](https://portal.azure.com/)na stronie integracja aplikacji **Jive** Znajdź sekcję **Zarządzanie** i wybierz pozycję **Logowanie jednokrotne**.
1. Na stronie **Wybierz metodę logowania jednokrotnego** wybierz pozycję **SAML**.
1. Na stronie **Konfigurowanie logowania jednokrotnego przy użyciu języka SAML** kliknij ikonę Edytuj/pióro, aby określić **podstawową konfigurację języka SAML** , aby edytować ustawienia.

   ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

1. W sekcji **Podstawowa konfiguracja języka SAML** wprowadź wartości dla następujących pól:

   a. W polu tekstowym **Adres URL logowania** wpisz adres URL, używając następującego wzorca: `https://<instance name>.jivecustom.com`

   b. W polu tekstowym **Identyfikator (identyfikator jednostki)** wpisz adres URL, używając następującego wzorca: 
   ```http
   https://<instance name>.jiveon.com
   ```

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zaktualizuj te wartości przy użyciu rzeczywistego identyfikatora i adresu URL logowania. Skontaktuj się z [zespołem obsługi klienta Jive](https://www.jivesoftware.com/services-support/) , aby uzyskać te wartości. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link do pobierania certyfikatu](common/metadataxml.png)

6. W sekcji **Konfigurowanie Jive** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

W tej sekcji utworzysz użytkownika testowego w Azure Portal o nazwie B. Simon.

1. W lewym okienku w Azure Portal wybierz pozycję **Azure Active Directory**, wybierz pozycję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.
1. Wybierz pozycję **nowy użytkownik** w górnej części ekranu.
1. We właściwościach **użytkownika** wykonaj następujące kroki:
   1. W polu **Nazwa** wprowadź wartość `B.Simon`.  
   1. W polu **Nazwa użytkownika** wprowadź wartość username@companydomain.extension . Na przykład `B.Simon@contoso.com`.
   1. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu **Hasło**.
   1. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji włączysz usługę B. Simon, aby korzystać z logowania jednokrotnego na platformie Azure przez przyznanie dostępu do usługi Jive.

1. W Azure Portal wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.
1. Na liście Aplikacje wybierz pozycję **Jive**.
1. Na stronie Przegląd aplikacji Znajdź sekcję **Zarządzanie** i wybierz pozycję **Użytkownicy i grupy**.

   ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

1. Wybierz pozycję **Dodaj użytkownika**, a następnie w oknie dialogowym **Dodawanie przypisania** wybierz pozycję **Użytkownicy i grupy** .

    ![Link Dodaj użytkownika](common/add-assign-user.png)

1. W oknie dialogowym **Użytkownicy i grupy** wybierz pozycję **B. Simon** z listy Użytkownicy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. Jeśli oczekujesz dowolnej wartości roli w potwierdzeniu SAML, w oknie dialogowym **Wybierz rolę** wybierz odpowiednią rolę dla użytkownika z listy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz** .

## <a name="configure-jive-sso"></a>Konfigurowanie logowania jednokrotnego Jive

1. Aby skonfigurować Logowanie jednokrotne na stronie **Jive** , zaloguj się do dzierżawy Jive jako administrator.

1. W menu u góry kliknij pozycję **SAML**.

    ![Zrzut ekranu przedstawia kartę SAML z włączonym zaznaczeniem.](./media/jive-tutorial/tutorial_jive_002.png)

    a. Na karcie **Ogólne** wybierz pozycję **włączone** .

    b. Kliknij przycisk **Zapisz wszystkie ustawienia SAML** .

1. Przejdź do karty **metadane dostawcy tożsamości** .

    ![Zrzut ekranu przedstawia zaznaczone metadane karty SAML I D P.](./media/jive-tutorial/tutorial_jive_003.png)

    a. Skopiuj zawartość pobranego pliku XML metadanych, a następnie wklej go do pola tekstowego **metadanych dostawcy tożsamości (dostawcy tożsamości)** .

    b. Kliknij przycisk **Zapisz wszystkie ustawienia SAML** .

1. Wybierz kartę **Mapowanie atrybutu użytkownika** .

    ![Zrzut ekranu przedstawia kartę SAML z wybranym MAPOWANIEm atrybutu użytkownika.](./media/jive-tutorial/tutorial_jive_004.png)

    a. W polu tekstowym **adres e-mail** skopiuj i wklej nazwę atrybutu wartość **poczty** .

    b. W polu tekstowym **imię i nazwisko** , skopiuj i wklej nazwę atrybutu **danej** wartościname.

    c. W polu **tekstowym nazwisko** , skopiuj i wklej nazwę atrybutu wartości **nazwisko** .

### <a name="create-jive-test-user"></a>Utwórz użytkownika testowego Jive

Celem tej sekcji jest utworzenie użytkownika o nazwie Britta Simon w Jive. Jive obsługuje automatyczne Inicjowanie obsługi użytkowników, która jest domyślnie włączona. Więcej szczegółów dotyczących konfigurowania automatycznego inicjowania obsługi użytkowników można znaleźć [tutaj](jive-provisioning-tutorial.md).

Jeśli trzeba ręcznie utworzyć użytkownika, należy skontaktować się z [zespołem obsługi klienta Jive](https://www.jivesoftware.com/services-support/) , aby dodać użytkowników z platformy Jive.

## <a name="test-sso"></a>Testuj Logowanie jednokrotne 

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD przy użyciu panelu dostępu.

Po kliknięciu kafelka Jive w panelu dostępu należy automatycznie zalogować się do Jive, dla którego skonfigurowano Logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [ Lista samouczków dotyczących integrowania aplikacji SaaS z usługą Azure Active Directory ](./tutorial-list.md)

- [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory? ](../manage-apps/what-is-single-sign-on.md)

- [Co to jest dostęp warunkowy w Azure Active Directory?](../conditional-access/overview.md)

- [Wypróbuj Jive z usługą Azure AD](https://aad.portal.azure.com/)

- [Co to jest kontrola sesji w Microsoft Cloud App Security?](/cloud-app-security/proxy-intro-aad)

- [Konfigurowanie aprowizacji użytkowników](jive-provisioning-tutorial.md)

- [Jak chronić Jive z zaawansowaną widocznością i kontrolkami](/cloud-app-security/proxy-intro-aad)