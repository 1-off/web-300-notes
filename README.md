# web-300-notes
## other notes
- https://www.terminal23.net/2022/05/31/awae-web-300-unused-prep-notes/
- https://rayhan0x01.github.io/web/2021/04/12/awae-web-300-oswe-guide-2021.html
- https://www.qa.com/course-catalogue/products/advanced-web-attack-exploitation-web-300-90-days-lab-access-qaawae90/
- https://infosecwriteups.com/cert-oswe-exam-review-and-tips-ft-no-developer-background-candidate-1dad7f545155
- https://github.com/topics/awae-prep
- https://hub.schellman.com/blog/oswe-review-and-exam-preparation-guide

## install pkg
- https://www.kali.org/tools/offsec-courses/
  - sudo apt install offsec-awae

## connect rdp 
```xfreerdp +nego +sec-rdp +sec-tls +sec-nla /d: /u: /p: /v:dnn /u:administrator /p:studentlab /size:1180x708```

## compile .net 
```C:\Windows\Microsoft.NET\Framework64\v4.0.30319\csc.exe test.cs```

## Cross-References
When analyzing and debugging more complex applications, one of the most useful features of a decompiler is the ability to find cross-references5 to a particular variable or function. We can use cross-references to better understand the code logic. For example, we can monitor the execution flow statically or set strategic breakpoints6 to debug and inspect the target application. We can demonstrate the effectiveness of cross-references in this process with a simple example.

Let's suppose that while studying our DotNetNuke target application, we noticed a few Base64-encoded values in the HTTP requests captured by Burp Suite. Since we would like to better understand where these values are decoded and processed within our target application, we could make the assumption that any functions that handle Base64-encoded values contain the word "base64".

We'll follow this assumption and start searching for these functions in dnSpy. For a thorough analysis we should open all the .NET modules loaded by the web application in our decompiler. However, for the purpose of this exercise, we'll only open the main DNN module, C:\inetpub\wwwroot\dotnetnuke\bin\DotNetNuke.dll, and search for the term "base64" within method names as shown in Figure 34.

- (DNN Corp., 2020), https://www.dnnsoftware.com/ ↩︎
- (0xd4d, 2020), https://github.com/0xd4d/dnSpy ↩︎
- (ICSharpCode , 2020), https://github.com/icsharpcode/ILSpy ↩︎
- (MicroSoft, 2021), https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/compiler-options/command-line-building-with-csc-exe ↩︎
- (Wikipedia, 2021), https://en.wikipedia.org/wiki/Cross-reference ↩︎
- (Wikipedia, 2019), https://en.wikipedia.org/wiki/Breakpoint ↩︎

# decompiling java classes
- http://java-decompiler.github.io/
