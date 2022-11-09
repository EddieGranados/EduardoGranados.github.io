###### [Home](https://eddiegranados.github.io/Eduardo_Granados/)

---

## User Optinos vs Project Options

#### User Options

- Global settings can be found in the User options tab

- The options provided in the User options tab will apply every time we open Burp Suite

- The User Options apply globally, but some of them can be overwritten in the project settings

- There are four main sub-sections of the User options tab:
  1. Connections - Controls how Burp makes connections to targets. A proxy can be set up for Burp Suite to connect through. This is very useful if we want to use Burp Suite through a [network pivot](https://www.geeksforgeeks.org/pivoting-moving-inside-a-network/)
  2. TLS - Allows for enabling and disabling various Transport Layer Security (TLS) options, or upload client certificates should a web app require one for connections
  3. Display - Customize Burp Suite appearance, configure options to do with the rendering engine in Repeater
  4. Misc - Has a variety of options like HotKeys, Proxy Interception, Updates

- With these options, we can customise our Burp install to suit our individual preferences


#### Project Options

- Project-specific settings can be found in the Project options tab.

- If you use the Burp Community version, Project Options will only apply to the current project and will reset every time Burp is closed.

- There are five sub-tabs here:
  1. Connections - Some options are equivalent to User options tab: these can be used to override the application-wide settings
  2. HTTP - Defines how Burp handles various aspects of the HTTP protocol.
  3. TLS - allows us to override application-wide TLS options, as well as showing us a list of public server certificates for sites that we have visited.
  4. Sessions - Provides us with options for handling sessions. It allows us to define how Burp obtains, saves, and uses session cookies that it receives from target sites. It also allows us to define macros which we can use to automate things such as logging into web applications (giving us an instant authenticated session, assuming we have valid credentials).
  5. Misc -  has some equivalent optinos from User options. Many of the options here are also only available if you have access to Burp Pro. There are a couple of options related to logging and the embedded browser.

---

## Burp Proxy

- Burp Proxy is a fundamental/important tool available in Burp Suite. It allows us to capture requests and responses  between ourselves and our target. With proxy active, a request is made to a website. The browser making the request will hang, and the request will appear in the proxy tab. We can then choose to forward or drop the request. These can then be manipulated or sent to other tools for further processing before being allowed to continue to their destination. Such as sending the request to one of the other Burp modules, copying it as  a cURL cmmand, saving it to a file, and many others. Our request will be captured and won't be allowed to continue to servers until we explicitly allow it through. We can choose to do the same with the response from the server, although this isn't active by default. This ability to intercept requests ultimately means that we can take complete control over our web traffic, an invaluable ability when it comes to testing web applications.

- By default Burp Suite will be logging requests made thorugh the proxy when the intercept is off. This can be very useful for going back and analysing prior requests, even if we didn't specifically capture them when they were made. Burp will also capture and log WebSocket communication which, can be helpful for analysing a web app. The logs can be viewed by going to the "HTTP history" and "WebSockets" sub-tabs. Any request captured here can be sent to other tools in the framework by right-clicking them and choosing "Send to...".

- There are Proxy specific options, which can be viewed in the "Options" sub-tab. These options give us a lot of control over how the proxy operates. By default, the proxy will not intercept sever responsese by default unless we explicitly ask it to on a per-request basis. We can override the default setting by selecting the "Intercept resposnes based on the following rules" checkbox and picking one or more rules. The "Or Request Was Intercepted" rule is good for catching responses to all requestes that were intercepted by the proxy. The "And URL Is in target scope" is another very good default rule. You can make your own rules for most of the Proxy options, so this one section where lookiing around and experimenting will be useful.

- "Match and Replace" section allows us to perform regexes on incoming and outgoing requests. For exmaple, you can automatically change your user agent to emulate a different web browser in outgoing request or remove all cookies being set in incoming requests. Again, you are free to make your own rules here.

---

## FoxyProxy

- There are two ways to proxy our traffic through Burp Suite:
  1. We could use the embedded broswer
  2. We can configure our local web browser to proxy our traffic through Burp

- The Burp Proxy work by opening a web interface on 127.0.0.1:8080, by default. We need to redirect all of our browser traffic through this port before we can start intercepting it with Burp. We can do this by altering our browser setting or using a browser extension called FoxyProxy. 

- There are two versions of of FoxyProxy: Basic and Standard. Standard gives us more control

- If we turn on FoxyProxy and using the proxy we created, our browser will start directing all of our traffic through 127.0.0.1:8080. If Burp Suite is not running, our browser will not be able to make requests when FoxyProxy is on. With FoxyProxy on, when can turn intercept option on for Burp. From there we can forward, drop, and edit requests. As well as send it to another tool or perform other actions by right clicking on the request and selecting an option from the right-click menu.

---

## Scoping and Targeting

- When Burp logs everything, it muddies up logs we may later wish to use. Setting a scope for the project allows us to define what gets proxied and logged. We can restrict Burp Suite to only target the web apps that we want to test. The easiest way to do this is by switching over to the "Target" tab, right-clicking our traget from our list on the left, then choose "Add To Scope". Burp will then ask us whether we want ot stop logging anything which isn't in the scope, most of the time we want to choose "yes" here.
- We can now check our scope by switching to the "Scope" sub-tab. The Scope sub-tab allows us to control what we are targeting by either Including or Excluding domains / IPs. This is a very powerful section, well worth taking the time to get accustomed to using it.
- We just chose to disable logging for out of scope traffic, but the proxy will still be intercepting everything. To turn this off, we need to go into the Proxy Options sub-tab and select "And URL Is in target scope" from the Intercept Client Request section. With this option selected, the proxy will completely ignore anything that isn't in the scope, cleaning up the traffic coming through Burp.

---

## Real World exmaple

- In a real-world web app pentest, we would test for a variety of things: one of which would be XSS, injecting a client-side script (usually JS) into a webpage in such a way that it executes. There are various kinds of XSS, such as "Reflected" XSS as it only affects the person making the web request.
  1. Typing `<script>alert("Successful XSS")</script>`, into email fields.