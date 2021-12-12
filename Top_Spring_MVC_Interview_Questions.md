# Top Spring MVC Interview Questions
Enlisted below are the most important interview questions for Spring MVC. Try to get the concept of each question, so that it will be very helpful in explaining to the interviewer.
Always try to give or correlate with examples of real-time applications. These are helpful for experienced professionals as well.

### Q #1) What is the Spring Framework and why is it so popular?
Answer: Spring Framework is a lightweight framework that is used to develop loosely coupled Java web applications.

It provides an inbuilt container, dependency injection, and MVC architecture. It is very popular as it allows decoupling, reusability, and provides design patterns like singleton, factory to reduce the code length, etc and also removes weak connections and has powerful integration with the third party.

### Q #2) What is Spring MVC?

Answer: Spring MVC follows the concept of Model, View, and Controller.

It is an instance of a controller that is shared alone and also helps to handle several requests that execute in Inversion of Control containers like interceptors and controllers. The Spring MVC pattern helps to separate the aspects of the application that includes input, business and presentation logic.

### Q #3) What are the core features of the Spring Framework?

Answer:

Core features of Spring MVC are:

    It is capable of effectively configuring the framework and classes as beans. It also divides the functional roles and responsibilities separately.
    It allows the definition of an unlimited controller method which makes the application highly adjustable and flexible.
    It provides good customization for handler mapping, binding, view resolution, and validations.
    It helps to transfer the model by using a map. It also provides supports for velocity, JSTL, JSP, and the user can customize locale and theme resolution.
    Spring has its own tag library which makes it more flexible and supports data binding, themes, beans having life cycle up to HTTP request.

### Q #4) Explain the concept of the Dispatcher Servlet.

Answer: Dispatcher Servlet is the main central servlet that handles all the incoming HTTP Request and Responses. It has integration with Spring IOC, and thus it allows to use all the features of Spring.

Once the dispatcher servlet receives a request, it forwards it to handler mapping for getting an appropriate controller, and now the controller will call the correct service method and again it will send it back to the dispatcher servlet.

Again, the servlet sends the request to the view resolver for getting the required view and then it sends the response to the client browser.

Code Example to show dispatcher servlet usage:
<web-app>
 
<display-name>Software Testing Help Web Application</display-name>
 
<servlet>
<servlet-name>SoftwareTestingHelp</servlet-name>
<servlet-class>org.Springframework.web.servlet.DispatcherServlet</servlet-class>
<load-on-startup>1</load-on-startup>
</servlet>
 
<servlet-mapping>
<servlet-name>SoftwareTestingHelp</servlet-name>
<url-pattern>/</url-pattern>
</servlet-mapping>
 
</web-app>

### Q #5) What is the use of ContextLoaderListner in Spring MVC?

Answer: It is used to read and parse the Spring configuration file and process the loading of beans in that configuration file.

Code Example:
<servlet>
    <servlet-name>Spring</servlet-name>
    <servlet-class>
        org.Springframework.web.servlet.DispatcherServlet
    </servlet-class>
      
     <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/applicationContext.xml</param-value>
    </init-param>
      
    <load-on-startup>1</load-on-startup>
</servlet>

### Q #6) Explain the front controller class of Spring MVC.

Answer: Front Controller is responsible to handle the entire incoming request of an application. In Spring MVC, dispatcher servlet acts as a front controller and handles the entire incoming requests.

### Q #7) Explain the function of @Autowired Annotation.

Answer: The @Autowired annotation is responsible for injecting a bean by its type along with the fields and methods. Thereby helping the Spring framework in resolving and injection of collaborating beans into our bean.

Example:
package com.SoftwareTestingHelp;
import org.Springframework.beans.factory.annotation.Autowired;
 
public class WordEditor {
   private SpellChecker sp;
 
   @Autowired
    public void setSp( SpellChecker sp ){
      this.sp = sp;
}
public SpellChecker getSp( ) {
   return sp;
}
public void spellCheck() {
  sp.checkSpelling();
 }
}

### Q #8) What is the function of Model Attribute annotation?

Answer: @ModelAttribute annotation plays a vital role in the Spring MVC framework. It is responsible for the binding method parameter with the respective model attribute and then shows it to the presentation page.

It also depends on what the developer is using in the model attribute annotation. If it is used at the method level, it indicates that the method is responsible to add attributes. When it is used as a parameter, it indicates that the parameter value should be retrieved from the model layer.

### Q #9) Explain the concept and function of the Path Variable.

Answer: The @PathVariable is an annotation that is used as a parameter in the handler method for extracting the value of the URI template.

Example:
@RequestMapping("/Login/{Lid}")
public String handleRequest(@PathVariable("Lid") String Login, Model map)
  {
}

Q #10) What is the function of annotation @RequestBody?

Answer: The @RequestBody annotation is responsible for binding the HTTP body request to a domain object. The incoming HTTP request is automatically de-serialized to the Java object by Spring with the help of the HTTP message converters.

Q #11) What is the function of annotation @ResponseBody?

Answer: When the @ResponseBody annotation is used in the MVC controller, it indicates that the developer needs to write a return type of declared method directly to the HTTP response body. Here, invoking of the model is not required and the view name is not interpreted by Spring.

Q #12) Why do we need View Resolver in Spring MVC and what is an internal view resolver in Spring?

Answer: View Resolver is responsible for rendering of models in the web browser. It does not require any specific view technologies like JSP, XML, and Velocity, etc.

The “InternalResourceViewResolver” is the internal view resolver in the Spring MVC.

There are some important view resolver’s in Spring MVC as mentioned below:

AbstractCachingViewResolver, XmlViewResolver, ResourceBundleViewResolver, UrlBasedViewResolver, InternalResourceViewResolver, FreeMarkerViewResolver, ContentNegotiatingViewResolver etc.

Q #13) Explain the working principle of the Spring MVC Framework.

Answer:

The below diagram will help in understanding the flow:

working principle of the Spring MVC Framework

As we know, it is based on the concept of Model, View, and Controller. Dispatcher Servlet receives the HTTP request depending on the servlet-mapping done in the web.xml.

Once the request is received by the Dispatcher Servlet, it sends the same request to the handler mapping to get the required controller class. Handler mapping is mentioned in the Spring configuration file.

Again the request is sent to the Controller, and the controller processes the request and method and sends back the required model view object to the dispatcher servlet.

Now, the request is sent to the view resolver by a servlet to get the presentation of the view page.

Lastly, the Dispatcher Servlet passes the model to the presentation page and the same is rendered to a client browser.

Q #14) What is the purpose of using BindingResults?

Answer: BindingResults is one of the interfaces in Spring that comes under the package org.Springframework.validation and is used to represent the binding results.

It plays a handy role in detecting errors from the submitted forms. It has a simple and easy invoking process. The developer has to keep in mind to put the binding result as a parameter just after the object validating.

Syntax:
@PostMapping("/client")
public String submitpage(@Valid NewUserPage newUserPage,
  BindingResult result, Model model) {
    if (result.hasErrors()) {
        return "uHome";
    }
    model.addAttribute("message", "Valid Page");
    return "uHome";
}

Here, the Spring framework will understand by reading the @Valid annotation and will try to find its validator.

Q #15) Explain the Model 1 and Model 2 architecture of Spring Framework.

Answer: These are the two important design models for developing web applications.

Model 1:

Here, the incoming request is handled by the servlet or a JSP. The servlet or JSP page is responsible for handling, processing the request, controlling the business logic, validation of data and lastly for generating the response.

It is simple and has an easy architecture and is mostly preferred by small-scale industries to develop simple applications.

Model 1 architeture of Spring

Model 2:

It is an important design model and is based on the Model View Controller concept. It is capable of separating the presentation and the logic section that manipulate the code.

Model 2 architeture of Spring

This model is robust, reliable, and avoids duplicity. It is good for building large and complex applications.

Q #16) Explain the function of the Spring MVC interceptor along with its usage.

Answer: Interceptors play a handy role in Spring MVC. They are used to intercept the request from the client, process the request at different times like before handling the request, after handling the request and after completion of the presentation part i.e. view page, etc.

It is also used from cross-cutting concerns and reduces the code handler repetition like a modification of global parameters in the model later, logging, etc.

Q #17) Is it allowed to use multiple Spring configuration files in Spring MVC?

Answer: Yes, Spring allows to have multiple configuration files in one application.

There are mainly two ways to achieve this and those ways are mentioned below:

(i) The Developer has to declare all the files in the web.xml using the parameter:
<servlet>
        <servlet-name>Spring</servlet-name>
        <servlet-class>
            org.Springframework.web.servlet.DispatcherServlet
        </servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>
                WEB-INF/Spring-dao-hibernate.xml,
           </param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
     
    <servlet-mapping>
        <servlet-name>Spring</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

(ii) The developer can import files into the existing configuration.
<beans>
    <import resource="Spring-dao-hibernate.xml"/>
</beans>

### Q #18) Explain the functions of the following annotations: @Component, @Controller, @Repository, @Service.

Answer: These are the important annotations in Spring MVC. Let’s take a look at them.

@Component: This annotation is responsible for converting a java class to the bean so that it can be recognized by Spring and used in the application context.

Syntax:

@Component
public class StudentDAOImpl implements StudentDAO {
    ...
}

@Repository: This annotation has the same use as that of @Component, but additionally it also considers the unchecked exceptions that can be used in Spring.

@Service: This annotation also converts java class to the bean, but it is better to use @service annotation than @component as it provides better specifications in a real-time application.

@Controller: If we want to make any class as a controller then we need to specify this annotation.

### Q #19) What is the role of Multipart resolver and when is it used?

Answer: Multipart Resolver is responsible for uploading the file in a web application. Two implementations for it are commonsMultipartResolver and StandardServletMultipartResolver.

### Q #20) How does Spring MVC provide support for validation?

Answer: Spring provides validation support in two ways – i.e Mainly by using JSR 303 annotation and Validator interface.

### Q #21) How is Localization achieved in the Spring MVC web applications?

Answer: Spring MVC is configured with LocaleResolver for supporting both internationalization and localization. For making this possible, some beans need to be registered as mentioned below.

SessionLocaleResolver – It takes the help of the already defined attributes in the user session for resolving locales.

Syntax:
<bean id="localeResolver"class="org.Springframework.web.servlet.i18n.SessionLocaleResolver">
    <property name="defaultLocale" value="en" />
</bean>

LocaleChangeInterceptor – It is useful when there is a parameter present in the incoming request.

```xml
Syntax:
<bean id="localeChangeInterceptor"class="org.Springframework.web.servlet.i18n.LocaleChangeInterceptor">
    <property name="paramName" value="lang" />
</bean>
 <beanclass="org.Springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping">
    <property name="interceptors">
        <list>
            <ref bean="localeChangeInterceptor" />
        </list>
    </property>
</bean>
```

###Q #22) Explain the concept of root application context in Spring MVC along with its loading process.

Answer: ContextLoaderListner is known as the root application context in Spring MVC and is used by the complete application.

It is a listener which helps the Spring MVC to bootstrap. It is used for loading and creating the applicationContext, to avoid writing explicit code. Spring bean is defined in the Application Context.

root application context in Spring MVC

Generally, dispatcherServlet is used for specifying the servlets in the Spring MVC.

### Q #23) What is the role of web.xml in Spring MVC and where is it located?

Answer: In web.xml, we configure the ContextLoaderListner. When we deploy the application, an instance is created by the Servlet container for ContextLoaderListner and then it leads to loading of the webApplicationContext.

### Q #24) Describe some return types of the controller method.

Answer: Some of the important return types of the controller methods include String, void, view, ModelAndView, Model, Map, HttpHeaders, etc.

### Q #25) On what basis is a view chosen in the rendering phase?

Answer: The ViewResolver in MVC architecture is responsible for choosing the required view.

When the controller sends the response to the dispatcher servlet, it sends the request to the view resolver for getting the correct view page which can be sent to a client web browser for presentation.

### Q #26) What is the main difference between @Requestparam and @PathVariable annotation?

Answer: These annotations are used for fetching information and data from an URL.

But, the main difference between them is, @Requestparam is only used to fetch query parameters and @PathVariable is used to fetch the complete URL.

### Q #27) Explain the concept of Aspect Oriented Programming?
 
Answer: AOP is an important part of Spring MVC Architecture. AOP is used for crosscutting concern and also for applications, validation of data, module logging, transaction management, authentication, and objects.

There are many parts of Aspect Oriented Programming. These are mentioned below:

    Aspect: Aspect is responsible for cross-cutting concerns like transaction management etc.
    Advice: It is basically an action and method that are executed and is also used for a specified join point.
    Pointcut: It is responsible for the execution of advice in terms of regular expressions.
    Joint Point: It is a point in the application for processes like exception handling, execution of the method, variable values change, etc.
    Advice Arguments: These arguments are used for passing of methods.

Conclusion

Hope, you would have got a complete overview of Spring MVC architecture from this article.

Here, we have covered the complete end to end flow of Spring MVC flow, Why we need Spring MVC? What are the different modules in MVC architecture? Features of Spring MVC and dispatcher servlet, the concept of the front controller, Listeners and its function, different types of annotation like @Autowired, @Controller, @RequestBody, etc.

We also learned more about View Resolver concept, AOP process and different types of the controller along with its uses. Localization and interceptors, different architecture of Spring framework and the concept of the binding result along with its uses.

We came to know what is a Spring container and its usage in Spring MVC? Usage of configurations files in Spring MVC, Path variable responsibilities and importance. etc Almost all the important and most expected Spring MVC interview questions are explained in this article.

However, it is always advisable to explain with code examples in the real-time application to satisfy the interviewer’s expectation. All the codes required for each and every concept are covered here for your easy understanding.

We wish you all the best!
