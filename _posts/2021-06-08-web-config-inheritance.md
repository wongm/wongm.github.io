---
layout: post
title: Caught out by Web.Config Inheritance
author: wongm
comments: true
tags: [c#, asp.net, web.config, web applications, dotnet]
excerpt_separator: <!--end_excerpt-->
---

When I'm doing web development in the ASP.NET ecosystem, Web.Config files are something I'm aware of, but have been lucky enough to avoid becoming an expert on. 

However after a recent clean up task broke our staging environment immediately after I landed it, I ended up deep in the details of how Web.Config values are used by the framework, and a feature I had hazy recollections of - Web.Config inheritance.

<!--end_excerpt-->

The first sign of something wrong was the [yellow screen of death](https://stackoverflow.com/questions/5443334/asp-net-hacking-the-yellow-screen-of-death) - aka the ASP.NET error page.

Unfortunately for me, the error was a major one - occurring at such a low level that none of our application logs included the details of what was going on. As a result, I had to log onto the web server, and check the IIS application logs. There I found my answer. 

```
w3wp.exe 
IIS APPPOOL\CompanyApplication 
ConfigurationErrorsException 

Could not load type 'Company.Security.AuthenticationModule' from assembly 'CompanyLib'. 
at System.Web.Configuration.ConfigUtil.GetType(String typeName, String propertyName, ConfigurationElement configElement, XmlNode node, Boolean checkAptcaBit, Boolean ignoreCase) 
at System.Web.Configuration.ConfigUtil.GetType(String typeName, String propertyName, ConfigurationElement configElement, Boolean checkAptcaBit) 
at System.Web.Configuration.Common.ModulesEntry.SecureGetType(String typeName, String propertyName, ConfigurationElement configElement) 
at System.Web.Configuration.Common.ModulesEntry..ctor(String name, String typeName, String propertyName, ConfigurationElement configElement)
at System.Web.HttpApplication.BuildIntegratedModuleCollection(List`1 moduleList) 
at System.Web.HttpApplication.GetModuleCollection(IntPtr appContext) 
at System.Web.HttpApplication.RegisterEventSubscriptionsWithIIS(IntPtr appContext, HttpContext context, MethodInfo[] handlers)
at System.Web.HttpApplication.InitSpecial(HttpApplicationState state, MethodInfo[] handlers, IntPtr appContext, HttpContext context)
at System.Web.HttpApplicationFactory.GetSpecialApplicationInstance(IntPtr appContext, HttpContext context) 
at System.Web.Hosting.PipelineRuntime.InitializeApplication(IntPtr appContext) 
Could not load type 'Company.Security.AuthenticationModule' from assembly 'CompanyLib'.
at System.RuntimeTypeHandle.GetTypeByName(String name, Boolean throwOnError, Boolean ignoreCase, Boolean reflectionOnly, StackCrawlMarkHandle stackMark, IntPtr pPrivHostBinder, Boolean loadTypeFromPartialName, ObjectHandleOnStack type)
at System.RuntimeTypeHandle.GetTypeByName(String name, Boolean throwOnError, Boolean ignoreCase, Boolean reflectionOnly, StackCrawlMark& stackMark, IntPtr pPrivHostBinder, Boolean loadTypeFromPartialName)
at System.RuntimeTypeHandle.GetTypeByName(String name, Boolean throwOnError, Boolean ignoreCase, Boolean reflectionOnly, StackCrawlMark& stackMark, Boolean loadTypeFromPartialName)
at System.RuntimeType.GetType(String typeName, Boolean throwOnError, Boolean ignoreCase, Boolean reflectionOnly, StackCrawlMark& stackMark)
at System.Type.GetType(String typeName, Boolean throwOnError, Boolean ignoreCase)
at System.Web.Compilation.BuildManager.GetType(String typeName, Boolean throwOnError, Boolean ignoreCase)
at System.Web.Configuration.ConfigUtil.GetType(String typeName, String propertyName, ConfigurationElement configElement, XmlNode node, Boolean checkAptcaBit, Boolean ignoreCase)  

https://devtest.company.com:443/index.aspx
/index.aspx 
```

But I was confused - `AuthenticationModule` was the class I had deleted, and was once referenced in the web.config as such.

```
<remove name="AuthorizationModule" />
<add name="AuthorizationModule" type="Company.Security.AuthenticationModule, CompanyLib" />
```

However as well as deleting the class, I had deleted the above web.config file reference to it, so the ASP.NET application should no longer be trying to load it - a fact that I'd proven in my local development environment. 

But why was it still trying to load the deleted class?

Turn out the staging server contains multiple copies of the same ASP.NET application, and the copy of it containing my change was in a subdirectory of another copy of the application, that *did* still had the `AuthenticationModule` class reference in the Web.Config reference in place. This meant that the ASP.NET application running in the subdirectory was getting the `AuthorizationModule` reference added back, which then failed
at runtime because the class had been deleted.

Luckily for me the fix was simple - readd the following line into my Web.Config file.

```
<remove name="AuthorizationModule" />
```

This line removes the `AuthorizationModule` reference inherited from the Web.Config file in the parent directory, and leaves the value blank, ensuring that the deleted `AuthenticationModule` class is never loaded.

**Footnote**

The presence of the "remove and then add" pattern for every module referenced in the Web.Config suggests that the developers who came before me encountered similar problems with their values being overridden due to inheritance, so they fixed it by clearing out the inherited value for every item, allowing for them to be explicitly declared in each subdirectory.

**Further reading**

* [https://weblogs.asp.net/jongalloway/10-things-asp-net-developers-should-know-about-web-config-inheritance-and-overrides](10 Things ASP.NET Developers Should Know About Web.config Inheritance and Overrides) by Jon Galloway
* [https://www.hanselman.com/blog/changing-aspnet-webconfig-inheritance-when-mixing-versions-of-child-applications](Changing ASP.NET web.config inheritance when mixing versions of child applications) by Scott Hanselman
* [https://www.eidias.com/blog/2019/2/21/how-to-nest-applications-in-iis](How to nest applications in IIS) by Mirek