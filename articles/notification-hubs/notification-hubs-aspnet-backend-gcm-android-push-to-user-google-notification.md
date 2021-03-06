---
title: 使用 Azure 通知中樞將通知傳送至特定 Android 應用程式
description: 了解如何使用 Azure 通知中樞將推播通知傳送至特定的使用者。
documentationcenter: android
services: notification-hubs
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: ae0e17a8-9d2b-496e-afd2-baa151370c25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc, devx-track-java
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 27a56a45845c1515b500a71528d3449b63c3f869
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323956"
---
# <a name="tutorial-send-push-notification-to-specific-android-users-using-azure-notification-hubs-and-google-cloud-messaging-deprecated"></a>教學課程：使用 Azure 通知中樞和 Google 雲端通訊 (已淘汰) 將推播通知傳送給特定 Android 使用者

> [!WARNING]
> Google 已於 2018 年 4 月 10 日淘汰 Google 雲端通訊 (GCM)。 GCM 伺服器和用戶端 API 會由其他項目取代，並且很快就會在 2019 年 5 月 29 日進行移除。 如需詳細資訊，請參閱 [GCM 和 FCM 常見問題集](https://developers.google.com/cloud-messaging/faq)。

[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

本教學課程將示範如何使用 Azure 通知中心，來將推播通知傳送到特定裝置上的特定應用程式使用者。 ASP.NET WebAPI 後端可用來驗證用戶端並產生通知，如指引文章[從您的應用程式後端註冊](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)中所示。 本教學課程以您在以下教學課程中建立的通知中樞為基礎：[教學課程：使用 Azure 通知中樞和 Google 雲端通訊將通知推送至 Android 裝置](notification-hubs-android-push-notification-google-gcm-get-started.md)。

在本教學課程中，您會執行下列步驟：

> [!div class="checklist"]
> * 建立會驗證使用者的後端 Web API 專案。  
> * 建立 Android 應用程式。
> * 測試應用程式

## <a name="prerequisites"></a>Prerequisites

完成[教學課程：使用 Azure 通知中樞和 Google 雲端通訊將通知推送至 Android 裝置](notification-hubs-android-push-notification-google-gcm-get-started.md)。

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>建立 Android 專案

下一個步驟是更新在[教學課程：使用 Azure 通知中樞和 Google 雲端通訊將通知推送至 Android 裝置](notification-hubs-android-push-notification-google-gcm-get-started.md)。

1. 開啟您的 `res/layout/activity_main.xml` 檔案，取代下列內容定義：

    這會加入新的 EditText 控制項，以便以使用者身分登入。 此外，也會針對將成為您傳送的通知一部分的使用者名稱標記加入欄位：

    ```xml
    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

    <EditText
        android:id="@+id/usernameText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/usernameHint"
        android:layout_above="@+id/passwordText"
        android:layout_alignParentEnd="true" />
    <EditText
        android:id="@+id/passwordText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/passwordHint"
        android:inputType="textPassword"
        android:layout_above="@+id/buttonLogin"
        android:layout_alignParentEnd="true" />
    <Button
        android:id="@+id/buttonLogin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/loginButton"
        android:onClick="login"
        android:layout_above="@+id/toggleButtonGCM"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="24dp" />
    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="WNS on"
        android:textOff="WNS off"
        android:id="@+id/toggleButtonWNS"
        android:layout_toLeftOf="@id/toggleButtonGCM"
        android:layout_centerVertical="true" />
    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="GCM on"
        android:textOff="GCM off"
        android:id="@+id/toggleButtonGCM"
        android:checked="true"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true" />
    <ToggleButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textOn="APNS on"
        android:textOff="APNS off"
        android:id="@+id/toggleButtonAPNS"
        android:layout_toRightOf="@id/toggleButtonGCM"
        android:layout_centerVertical="true" />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessageTag"
        android:layout_below="@id/toggleButtonGCM"
        android:layout_centerHorizontal="true"
        android:hint="@string/notification_message_tag_hint" />
    <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_below="@+id/editTextNotificationMessageTag"
        android:layout_centerHorizontal="true"
        android:hint="@string/notification_message_hint" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:onClick="sendNotificationButtonOnClick"
        android:layout_below="@+id/editTextNotificationMessage"
        android:layout_centerHorizontal="true" />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:id="@+id/text_hello"
    />  
    </RelativeLayout>
    ```
2. 開啟 `res/values/strings.xml` 檔案，並以下列幾行程式碼取代 `send_button` 定義，這幾行程式碼可重新定義 `send_button` 的字串並新增其他控制項的字串：

    ```xml
    <string name="usernameHint">Username</string>
    <string name="passwordHint">Password</string>
    <string name="loginButton">1. Sign in</string>
    <string name="send_button">2. Send Notification</string>
    <string name="notification_message_hint">Notification message</string>
    <string name="notification_message_tag_hint">Recipient username</string>
    ```

    您的 `main_activity.xml` 圖形版面配置現在應如下圖所示：

    ![應用程式的螢幕擷取畫面，其中包含使用者名稱、密碼、收件者和訊息的方塊，以及用來登入和傳送通知的按鈕。][A1]
3. 在與 `MainActivity` 類別相同的套件中，建立名為 `RegisterClient` 的新類別。 將下列程式碼使用於新的類別檔案。

    ```java
    import java.io.IOException;
    import java.io.UnsupportedEncodingException;
    import java.util.Set;

    import org.apache.http.HttpResponse;
    import org.apache.http.HttpStatus;
    import org.apache.http.client.ClientProtocolException;
    import org.apache.http.client.HttpClient;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.client.methods.HttpPut;
    import org.apache.http.client.methods.HttpUriRequest;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.DefaultHttpClient;
    import org.apache.http.util.EntityUtils;
    import org.json.JSONArray;
    import org.json.JSONException;
    import org.json.JSONObject;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.util.Log;

    public class RegisterClient {
        private static final String PREFS_NAME = "ANHSettings";
        private static final String REGID_SETTING_NAME = "ANHRegistrationId";
        private String Backend_Endpoint;
        SharedPreferences settings;
        protected HttpClient httpClient;
        private String authorizationHeader;

        public RegisterClient(Context context, String backendEndpoint) {
            super();
            this.settings = context.getSharedPreferences(PREFS_NAME, 0);
            httpClient =  new DefaultHttpClient();
            Backend_Endpoint = backendEndpoint + "/api/register";
        }

        public String getAuthorizationHeader() {
            return authorizationHeader;
        }

        public void setAuthorizationHeader(String authorizationHeader) {
            this.authorizationHeader = authorizationHeader;
        }

        public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
            String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

            JSONObject deviceInfo = new JSONObject();
            deviceInfo.put("Platform", "gcm");
            deviceInfo.put("Handle", handle);
            deviceInfo.put("Tags", new JSONArray(tags));

            int statusCode = upsertRegistration(registrationId, deviceInfo);

            if (statusCode == HttpStatus.SC_OK) {
                return;
            } else if (statusCode == HttpStatus.SC_GONE){
                settings.edit().remove(REGID_SETTING_NAME).commit();
                registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                statusCode = upsertRegistration(registrationId, deviceInfo);
                if (statusCode != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            } else {
                Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                throw new RuntimeException("Error upserting registration");
            }
        }

        private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                throws UnsupportedEncodingException, IOException,
                ClientProtocolException {
            HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
            request.setEntity(new StringEntity(deviceInfo.toString()));
            request.addHeader("Authorization", "Basic "+authorizationHeader);
            request.addHeader("Content-Type", "application/json");
            HttpResponse response = httpClient.execute(request);
            int statusCode = response.getStatusLine().getStatusCode();
            return statusCode;
        }

        private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
            if (settings.contains(REGID_SETTING_NAME))
                return settings.getString(REGID_SETTING_NAME, null);

            HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
            request.addHeader("Authorization", "Basic "+authorizationHeader);
            HttpResponse response = httpClient.execute(request);
            if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                throw new RuntimeException("Error creating Notification Hubs registrationId");
            }
            String registrationId = EntityUtils.toString(response.getEntity());
            registrationId = registrationId.substring(1, registrationId.length()-1);

            settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

            return registrationId;
        }
    }
    ```

    為註冊推播通知，此元件會實作連絡應用程式後端所需的 REST 呼叫。 它也會在本機儲存通知中心所建立的 *registrationIds* ，如 [從您的應用程式後端註冊](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend)中的詳細說明。 當您按一下 [登入] 按鈕時，系統便會使用儲存在本機儲存體中的授權權杖。
4. 在您的類別中，針對 `NotificationHub` 移除或註解排除您的私用欄位，並對 `RegisterClient` 類別新增一個欄位，以及對 ASP.NET 後端端點新增一個字串。 請務必將 `<Enter Your Backend Endpoint>` 取代為您先前取得的實際後端端點。 例如： `http://mybackend.azurewebsites.net` 。

    ```java
    //private NotificationHub hub;
    private RegisterClient registerClient;
    private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
    ```

5. 在 `MainActivity` 類別的 `onCreate` 方法中，移除或註解排除 `hub` 欄位的初始化和 `registerWithNotificationHubs` 方法的呼叫。 然後加入程式碼以初始化 `RegisterClient` 類別的執行個體。 此方法應包含下列程式碼行：

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        mainActivity = this;
        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
        gcm = GoogleCloudMessaging.getInstance(this);

        //hub = new NotificationHub(HubName, HubListenConnectionString, this);
        //registerWithNotificationHubs();

        registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

        setContentView(R.layout.activity_main);
    }
    ```
6. 在 `MainActivity` 類別中，刪除或註解排除整個 `registerWithNotificationHubs` 方法。 本教學課程不會使用此方法。
7. 將下列 `import` 陳述式新增至 `MainActivity.java` 檔案。

    ```java
    import android.util.Base64;
    import android.view.View;
    import android.widget.EditText;

    import android.widget.Button;
    import android.widget.ToggleButton;
    import java.io.UnsupportedEncodingException;
    import android.content.Context;
    import java.util.HashSet;
    import android.widget.Toast;
    import org.apache.http.client.ClientProtocolException;
    import java.io.IOException;
    import org.apache.http.HttpStatus;

    import android.os.AsyncTask;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.DefaultHttpClient;

    import android.app.AlertDialog;
    import android.content.DialogInterface;
    ```
8. 將 onStart 方法的程式碼取代為下列程式碼：

    ```java
    super.onStart();
    Button sendPush = (Button) findViewById(R.id.sendbutton);
    sendPush.setEnabled(false);
    ```
9. 然後，新增下列方法來處理 [登入] 按鈕 click 事件及傳送推播通知。

    ```java
    public void login(View view) throws UnsupportedEncodingException {
        this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

        final Context context = this;
        new AsyncTask<Object, Object, Object>() {
            @Override
            protected Object doInBackground(Object... params) {
                try {
                    String regid = gcm.register(NotificationSettings.SenderId);
                    registerClient.register(regid, new HashSet<String>());
                } catch (Exception e) {
                    DialogNotify("MainActivity - Failed to register", e.getMessage());
                    return e;
                }
                return null;
            }

            protected void onPostExecute(Object result) {
                Button sendPush = (Button) findViewById(R.id.sendbutton);
                sendPush.setEnabled(true);
                Toast.makeText(context, "Logged in and registered.",
                        Toast.LENGTH_LONG).show();
            }
        }.execute(null, null, null);
    }

    private String getAuthorizationHeader() throws UnsupportedEncodingException {
        EditText username = (EditText) findViewById(R.id.usernameText);
        EditText password = (EditText) findViewById(R.id.passwordText);
        String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
        basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
        return basicAuthHeader;
    }

    /**
        * This method calls the ASP.NET WebAPI backend to send the notification message
        * to the platform notification service based on the pns parameter.
        *
        * @param pns     The platform notification service to send the notification message to. Must
        *                be one of the following ("wns", "gcm", "apns").
        * @param userTag The tag for the user who will receive the notification message. This string
        *                must not contain spaces or special characters.
        * @param message The notification message string. This string must include the double quotes
        *                to be used as JSON content.
        */
    public void sendPush(final String pns, final String userTag, final String message)
            throws ClientProtocolException, IOException {
        new AsyncTask<Object, Object, Object>() {
            @Override
            protected Object doInBackground(Object... params) {
                try {

                    String uri = BACKEND_ENDPOINT + "/api/notifications";
                    uri += "?pns=" + pns;
                    uri += "&to_tag=" + userTag;

                    HttpPost request = new HttpPost(uri);
                    request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                    request.setEntity(new StringEntity(message));
                    request.addHeader("Content-Type", "application/json");

                    HttpResponse response = new DefaultHttpClient().execute(request);

                    if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                        DialogNotify("MainActivity - Error sending " + pns + " notification",
                            response.getStatusLine().toString());
                        throw new RuntimeException("Error sending notification");
                    }
                } catch (Exception e) {
                    DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                    return e;
                }

                return null;
            }
        }.execute(null, null, null);
    }
    ```

    [登入] 按鈕的 `login` 處理常式會使用輸入使用者名稱和密碼 (這代表驗證結構描述使用的任何權杖) 產生基本驗證權杖，然後使用 `RegisterClient` 呼叫後端進行註冊。

    `sendPush` 方法會呼叫後端，以根據使用者標記觸發使用者的安全通知。 `sendPush` 鎖定目標的平台通知服務取決於傳入的 `pns` 字串。

10. 將下列 `DialogNotify` 方法新增至 `MainActivity` 類別。

    ```java
    protected void DialogNotify(String title, String message)
    {
        AlertDialog alertDialog = new AlertDialog.Builder(MainActivity.this).create();
        alertDialog.setTitle(title);
        alertDialog.setMessage(message);
        alertDialog.setButton(AlertDialog.BUTTON_NEUTRAL, "OK",
                new DialogInterface.OnClickListener() {
                    public void onClick(DialogInterface dialog, int which) {
                        dialog.dismiss();
                    }
                });
        alertDialog.show();
    }
    ```
11. 在 `MainActivity` 類別中，更新 `sendNotificationButtonOnClick` 方法以透過使用者選取的平台通知服務呼叫 `sendPush` 方法，如下所示。

    ```java
    /**
    * Send Notification button click handler. This method sends the push notification
    * message to each platform selected.
    *
    * @param v The view
    */
    public void sendNotificationButtonOnClick(View v)
            throws ClientProtocolException, IOException {

        String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                .getText().toString();
        String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                .getText().toString();

        // JSON String
        nhMessage = "\"" + nhMessage + "\"";

        if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
        {
            sendPush("wns", nhMessageTag, nhMessage);
        }
        if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
        {
            sendPush("gcm", nhMessageTag, nhMessage);
        }
        if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
        {
            sendPush("apns", nhMessageTag, nhMessage);
        }
    }
    ```
12. 在 `build.gradle` 檔案中，將以下這一行新增至 `buildTypes` 區段後面的 `android` 區段。

    ```java
    useLibrary 'org.apache.http.legacy'
    ```
13. 建置專案。

## <a name="test-the-app"></a>測試應用程式

1. 在使用 Android Studio 的裝置或模擬器上執行應用程式。
2. 在 Android 應用程式中，輸入使用者名稱和密碼。 兩者必須是相同的字串值，而且不得包含空格或特殊字元。
3. 在 Android 應用程式中，按一下 [登入]。 等待快顯訊息出現，指出 [ **已登入並註冊**]。 這會啟用 [傳送通知] 按鈕。

    ![應用程式的螢幕擷取畫面。 確認已顯示使用者已登入並註冊的快顯訊息，而且 [傳送通知] 按鈕已開啟。][A2]
4. 按一下切換按鈕，啟用您已執行應用程式並註冊使用者的所有平台。
5. 輸入會收到通知訊息的使用者名稱。 該使用者必須在目標裝置上註冊通知。
6. 輸入做為推播通知訊息的訊息，以便使用者接收。
7. 按一下 [ **傳送通知**]。  每個具有相符使用者名稱標記之註冊的裝置都會收到推播通知。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已學會如何針對具有與其註冊相關聯標記的使用者，將通知推送至這些特定使用者。 若要了解如何推送以位置為基礎的通知，請繼續進行下列教學課程：

> [!div class="nextstepaction"]
>[推送以位置為基礎的通知](notification-hubs-push-bing-spatial-data-geofencing-notification.md)

[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
