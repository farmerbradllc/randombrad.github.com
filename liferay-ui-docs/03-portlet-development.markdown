# Portlet Development

In this chapter we will create and deploy a simple portlet using the
Plugins SDK. It will allow a customized greeting to be saved in the
portlet's preferences, and then display it whenever the portlet is
viewed. Finally we will add friendly URL mapping to the portlet to clean
up its URLs.

In developing your own portlets you are free to use any framework you
prefer, such as Struts, Spring MVC, or JSF. For this portlet we will use
the Liferay MVCPortlet framework as it is simple, lightweight, and easy
to understand.

Additionally, Liferay allows for the consuming of PHP and Ruby
applications as portlets, so you do not need to be a Java developer in
order to take advantage of Liferay's built-in features (such as user
management, communities, page building and content management). You can
use the Plugins SDK to deploy your PHP or Ruby application as a portlet,
and it will run seamlessly inside of Liferay. There are plenty of
examples of this; to see them, check out the directory *plugins/trunk*
from Liferay's public Subversion repository.

## Creating a Portlet

Creating portlets with the Plugins SDK is extremely simple. As noted
before, there is a *portlets*folder inside of the Plugins SDK folder.
This is where your portlet projects will reside. To create a new
portlet, first decide what its name is going to be. You need both a
project name (without spaces) and a display name (which can have
spaces). When you have decided on your portlet's name, you are ready to
create the project. For the greeting portlet, the project name is
“my-greeting”, and the portlet title is “My Greeting”. Navigate to the
*portlets* directory in the terminal and enter the following command
(Linux and Mac OS X):

./create.sh my-greeting "My Greeting"

On Windows enter the following instead:

create.bat my-greeting "My Greeting"

You should get a BUILD SUCCESSFUL message from Ant, and there will now
be a new folder inside of the *portlets*folder in your Plugins SDK. This
folder is your new portlet project. This is where you will be
implementing your own functionality. Notice that the Plugins SDK
automatically appends “-portlet” to the project name when creating this
folder.

Alternatively, if you will not be using the Plugins SDK to house your
portlet projects, you can copy your newly created portlet project into
your IDE of choice and work with it there. If you do this, you may need
to make sure the project references some .jar files from your Liferay
installation, or you may get compile errors. Since the ant scripts in
the Plugins SDK do this for you automatically, you don't get these
errors when working with the Plugins SDK.

To resolve the dependencies for portlet projects, see the class path
entries in the *build-common.xml* file in the Plugins SDK project. You
will be able to determine from the *plugin.classpath* and
*portal.classpath entries*which .jar files are necessary to build your
newly created portlet project.

\

![image](../../images/03-portlet-development_html_5c790363.png)**Tip:** If you are
using a source control system such as Subversion, CVS, Mercurial, Git,
etc. this might be a good moment to do an initial check in of your
changes. After building the plugin for deployment several additional
files will be generated that should not be handled by the source control
system.

\
\

### Deploying the Portlet

Liferay provides a mechanism called autodeploy that makes deploying
portlets (and any other plugin type) a breeze. All you need to do is
drop a WAR file into a directory and the portal will take care of making
any necessary changes specific to Liferay and then deploy it to the
application server. This will be the method used throughout this guide.

\

![image](../../images/03-portlet-development_html_5c790363.png)**Tip:** Liferay
supports a wide variety of application servers. Many of them, such as
Tomcat or Jboss, provide a simple way to deploy web applications by just
copying a file into a folder and Liferay's autodeploy mechanism makes
use of that possibility. You should be aware though that some
application servers, such as Websphere or Weblogic require the use of
specific tools to deploy web applications, so Liferay's autodeploy
process won't work for them.

\
\

Open a terminal window in your *portlets/my-greeting-portlet* directory
and enter this command:

ant deploy

You should get a BUILD SUCCESSFUL message, which means that your portlet
is now being deployed. If you switch to the terminal window running
Liferay, and wait for a few seconds, you should see the message “1
portlet for my-greeting-portlet is available for use”. If not, something
is wrong and you should double-check your configuration.

Go to your web browser and log in to the portal as explained earlier.
Then hover over *Add* at the top of the page, and click on *More*.
Select the *Sample* category, and then click *Add* next to *My
Greeting*. Your portlet should appear in the page below.
Congratulations, you've just created your first portlet!

## Anatomy of a Portlet

A portlet project is made up at a minimum of three components:

1.  Java Source

2.  Configuration files

3.  Client-side files (\*.jsp, \*.css, \*.js, graphics, etc.)

When using Liferay's Plugins SDK these files are stored in a standard
directory structure which looks like the following:

\
\

/PORTLET-NAME/

build.xml

/docroot/

/css/

/js/

/WEB-INF/\
 /src/ (not created by default)

liferay-display.xml

liferay-plugin-package.properties

liferay-portlet.xml

portlet.xml

web.xml

icon.png

view.jsp

The portlet we just created is a fully functional portlet which can be
deployed to your Liferay instance.

New portlets are configured by default to use the MVCPortlet framework,
a very light framework that hides part of the complexity of portlets and
makes the most common operations easier. MVCPortlet uses separate JSPs
for each page in the portlet.

Portlets created in the SDK are configured by default to use the
MVCPortlet framework, a very light framework that hides part of the
complexity of portlets and makes the most common operations easier.
MVCPortlet uses separate JSPs for each page in the portlet. MVCPortlet
uses by default a JSP with the mode name for each of the registered
portlet modes. For example edit.jsp for the edit mode, help.jsp for the
help mode, etc.

The **Java Source** is stored in the *docroot/WEB-INF/src* folder

The **Configuration Files** are stored in the *docroot/WEB-INF* folder.
The standard JSR-286 portlet configuration file *portlet.xml* is here,
as well as three Liferay-specific configuration files. The
Liferay-specific configuration files are completely optional, but are
important if your portlets are going to be deployed on a Liferay Portal
server.

*liferay-display.xml*: This file describes what category the portlet
should appear under in the *Add*menu in the dockbar (the horizontal bar
that appear at the top of the page to all logged in users).

*liferay-portlet.xml*: This file describes some optional
Liferay-specific enhancements for JSR-286 portlets that are installed on
a Liferay Portal server. For example, you can set whether a portlet is
instanceable, which means that you can place more than one portlet
instance on a page, and each one will have its own separate data. Please
see the DTD for this file for further details, as there are too many
settings to list here. The DTD may be found in the *definitions* folder
in the Liferay source code.

*liferay-plugin-package.properties*: This file describes the plugin to
Liferay's hot deployer. One of the things that can be configured in this
file is dependency .jars. If a portlet plugin has dependencies on
particular .jar files that already come with Liferay, you can specify
them in this file and the hot deployer will modify the .war file on
deployment to copy those .jars inside. That way you don't have to
include the .jars yourself and the .war will be lighter.

**Client Side Files****are the .jsp, .css, and JavaScript files that you
write to implement your portlet's user interface. These files should go
in the docroot folder somewhere—either in the root of the folder or in a
folder structure of their own. Remember that with portlets you are only
dealing with a portion of the HTML document that is getting returned to
the browser. Any HTML code you have in your client side files should be
free of global tags such as `<html>`{.western} or `<head>`{.western}.
Additionally, all CSS classes and element IDs must be namespaced to
prevent conflicts with other portlets. Liferay provides tools (a taglib
and API methods) to generate the namespace that you should use.

### A Closer Look at the My Greeting Portlet

If you are new to portlet development, this section will take a closer
look at the configuration options of a portlet.

**docroot/WEB-INF/portlet.xml**

When using the Plugins SDK, the default content of the portlet
descriptor is as follows:

<portlet\>

<portlet-name\>my-greeting</portlet-name\>

<display-name\>My Greeting</display-name\>

<portlet-class\>com.liferay.util.bridges.mvc.MVCPortlet</portlet-class\>

<init-param\>

<name\>view-jsp</name\>

<value\>/view.jsp</value\>

</init-param\>

<expiration-cache\>0</expiration-cache\>

<supports\>

<mime-type\>text/html</mime-type\>

</supports\>

<portlet-info\>

<title\>My Greeting</title\>

<short-title\>My Greeting</short-title\>

<keywords\>My Greeting</keywords\>

</portlet-info\>

<security-role-ref\>

<role-name\>administrator</role-name\>

</security-role-ref\>

<security-role-ref\>

<role-name\>guest</role-name\>

</security-role-ref\>

<security-role-ref\>

<role-name\>power-user</role-name\>

</security-role-ref\>

<security-role-ref\>

<role-name\>user</role-name\>

</security-role-ref\>

</portlet\>

Here is a basic summary of what each of the elements represents:

portlet-name

The portlet-name element contains the canonical name of the portlet.
Each portlet name is unique within the portlet application (that is,
within the portlet plugin). This is also referred within Liferay Portal
as the portlet id

display-name

The display-name type contains a short name that is intended to be
displayed by tools. It is used by display-name elements. The display
name need not be unique.

portlet-class

The portlet-class element contains the fully qualified class name that
will handle invocations to the portlet.

init-param

The init-param element contains a name/value pair as an initialization
param of the portlet.

expiration-cache

Expiration-cache defines expiration-based caching for this portlet. The
parameter indicates the time in seconds after which the portlet output
expires. -1 indicates that the output never expires.

supports

The supports element contains the supported mime-type. Supports also
indicates the portlet modes a portlet supports for a specific content
type. All portlets must support the view mode. \
The concept of “portlet modes” is defined by the portlet specification.
Modes are used to separate certain views of the portlet from others.
What is special about portlet modes is that the portal knows about them
and can provide generic ways to navigate between portlet modes (for
example through links in the box surrounding the portlet when it is
added to a page). For that reason they are useful for operations that
are common to all or most portlets. The most common usage is to create
an edit screen where each user can specify personal preferences for the
portlet.

portlet-info

Portlet-info defines portlet information.

security-role-ref

The security-role-ref element contains the declaration of a security
role reference in the code of the web application. Specifically in
Liferay, the role-name references which roles can access the portlet.

\

**docroot/WEB-INF/liferay-portlet.xml** - In addition to the standard
`portlet.xml`{.western} options, there are optional Liferay-specific
enhancements for Java Standard portlets that are installed on a Liferay
Portal server. By default, Plugins SDK sets the contents of this
descriptor to the following:

<liferay-portlet-app\>

<portlet\>

<portlet-name\>my-greeting</portlet-name\>

<icon\>/icon.png</icon\>

<instanceable\>false</instanceable\>

<header-portlet-css\>/css/main.css</header-portlet-css\>

<footer-portlet-javascript\>/js/main.js</footer-portlet-javascript\>

<css-class-wrapper\>my-greeting-portlet</css-class-wrapper\>

</portlet\>

<role-mapper\>

<role-name\>administrator</role-name\>

<role-link\>Administrator</role-link\>

</role-mapper\>

<role-mapper\>

<role-name\>guest</role-name\>

<role-link\>Guest</role-link\>

</role-mapper\>

<role-mapper\>

<role-name\>power-user</role-name\>

<role-link\>Power User</role-link\>

</role-mapper\>

<role-mapper\>

<role-name\>user</role-name\>

<role-link\>User</role-link\>

</role-mapper\>

</liferay-portlet-app\>

Here is a basic summary of what some of the elements represents.

portlet-name

The portlet-name element contains the canonical name of the portlet.
This needs to be the same as the portlet-name given in portlet.xml

icon

Path to icon image for this portlet

instanceable

Indicates if multiple instances of this portlet can appear on the same
page.

header-portlet-css

The path to the .css file for this portlet to be included in the <head\>
of the page

footer-portlet-javascript

The path to the .js file for this portlet, to be included at the end of
the page before </body\>

\
\

There are many more elements that you should be aware of for more
advanced development. Please see the DTD for this file in the
*definitions* folder in the Liferay source code for more information.

## Writing the My Greeting Portlet

Now that you are familiar with the structure of a portlet, it's time to
actually make it do something useful. Our portlet will have two pages.
*view.jsp* will display the greeting and provide a link to the edit
page. *Edit.jsp* will show a form with a text field allowing the
greeting to be changed, along with a link back to the view page.
MVCPortlet class will handle the rendering of our JSPs, so for this
example we won't have to write a single Java class.

First, we don't want multiple greetings on the same page, so we are
going to make the My Greeting portlet non-instanceable. To do this, edit
*liferay-portlet.xml* and change the value of the element *instanceable*
from true to false so that it looks like this:

<instanceable\>false</instanceable\>

Next, we will create our JSP templates. Start by editing *view.jsp* and
replacing its current contents with the following:

<%@ taglib uri="http://java.sun.com/portlet\_2\_0" prefix="portlet" %\>

<%@ page import="javax.portlet.PortletPreferences" %\>

<portlet:defineObjects /\>

\
\

<%

PortletPreferences prefs = renderRequest.getPreferences();

String greeting = (String)prefs.getValue(

"greeting", "Hello! Welcome to our portal.");

%\>

\
\

<p\><%= greeting %\></p\>

\
\

<portlet:renderURL var="editGreetingURL"\>

<portlet:param name="jspPage" value="/edit.jsp" /\>

</portlet:renderURL\>

\
\

<p\><a href="<%= editGreetingURL %\>"\>Edit greeting</a\></p\>

Next, create *edit.jsp* in the same directory as *view.jsp*with the
following content:

<%@ taglib uri="http://java.sun.com/portlet\_2\_0" prefix="portlet" %\>

<%@ taglib uri="http://liferay.com/tld/aui" prefix="aui" %\>

<%@ page import="javax.portlet.PortletPreferences" %\>

<portlet:defineObjects /\>

\
\

<%

PortletPreferences prefs = renderRequest.getPreferences();

\
\

String greeting = renderRequest.getParameter("greeting");

\
\

if (greeting != null) {

prefs.setValue("greeting", greeting);

prefs.store();

%\>

<p\>Greeting saved successfully!</p\>

<%

}

%\>

\
\

<%

greeting = (String)prefs.getValue(

"greeting", "Hello! Welcome to our portal.");

%\>

\
\

<portlet:renderURL var="editGreetingURL"\>

<portlet:param name="jspPage" value="/edit.jsp" /\>

</portlet:renderURL\>

\
\

<aui:form action="<%= editGreetingURL %\>" method="post"\>

<aui:input label="greeting" name="greeting" type="text" value="<%=
greeting %\>" /\>

<aui:button type="submit" /\>

</aui:form\>

\
\

<portlet:renderURL var="viewGreetingURL"\>

<portlet:param name="jspPage" value="/view.jsp" /\>

</portlet:renderURL\>

\
\

<p\><a href="<%= viewGreetingURL %\>"\>&larr; Back</a\></p\>

Deploy the portlet again by entering the command **ant deploy** in your
*my-greeting-portlet* folder. Go back to your web browser and refresh
the page; you should now be able to use the portlet to save and display
a custom greeting.

![image](../../images/03-portlet-development_html_5c790363.png)**Tip:** If your
portlet deployed successfully, but you don't see any changes in your
browser after refreshing the page, Tomcat may have failed to rebuild
your JSPs. Simply delete the *work* folder in
*liferay-portal-[version]/tomcat-[tomcat-version]* and refresh the page
again to force them to be rebuilt.

There are a few important details to notice in this implementation.
First, the links between pages are created using the
`<portlet:renderURL>`{.western} tag, which is defined by the
`http://java.sun.com/portlet_2_0`{.western} tag library. These URLs have
only one parameter named *jspPage*, which is used by MVCPortlet to
determine which JSP to render for each request. You must always use
taglibs to generate URLs to your portlet. This restriction exists
because the portlet does not own the whole page, only a fragment of it,
so the URL must always go to the portal who is responsible for
rendering, not only your portlet but also any others that the user might
put in the page. The portal will be able to interpret the taglib and
create a URL with enough information to be able to render the whole
page.

Second, notice that the form in *edit.jsp* has the prefix
`aui`{.western}, signifying that it is part of the Alloy UI tag library.
Alloy greatly simplifies the code required to create nice looking and
accessible forms, by providing tags that will render both the label and
the field at once. You can also use regular HTML or any other taglibs to
create forms based on your own preferences.

Another JSP tag that you may have noticed is
`<portlet:defineObjects/>`{.western}. The portlet specification defined
this tag in order to be able to insert into the JSP a set of implicit
variables that are useful for portlet developers such as renderRequest,
portletConfig, portletPreferences, etc.

**One word of warning** about the portlet we have just built. For the
purpose of making this example as simple and easy to follow as possible,
we have cheated a little bit. The portlet specification does not allow
to set preferences from a JSP, because they are executed in what is
known as the render state. There are good reasons for this restriction,
that are explained in the next section.

## Understanding the Two phases of Portlet Execution

One of the characteristics of portlet development that confuses most
developers used to regular servlet development or who are used to other
environments such as PHP, Python or Ruby is the need for two phases. The
good news is that once you get used to them they become simple and
useful.

The reason why two phases are needed is because a portlet does not own a
whole HTML page, it only generates a fragment of it. The portal that
holds the portlet is the one responsible for generating the page by
invoking one or several portlets and adding some additional HTML around
them. Usually, when the user interacts with the page, for example by
clicking a link or a button, she's doing it within a specific portlet.
The portal must forward the action performed by the user to that portlet
and after that it must render the whole page, showing the content of
that portlet, which may have changed, and also the content of the other
portlets. For the other portlets in the page which have not been invoked
by the user, what the portal does to get their content is to repeat the
last invocation again (since it assumes it will yield the same result).

Now imagine this scenario: we have a page with two portlets, a
navigation portlet and a shopping portlet. A user comes to the page and
does the following:

1.  Load the page

2.  Clicks a button on the shopping portlet that automatically charges
    an amount on her credit card and starts a process to ship her the
    product she just bought. After this operation the portal also
    invokes the navigation portlet with its default view.

3.  Click a link in the navigation portlet which causes the content of
    the portlet to change. After that the portal must also show the
    content of the shopping portlet, so it repeats the last action (the
    one in which the user clicked a button), which causes a new charge
    on the credit card and the start of a new shipping process.

I guess that by now you can tell that this is not right. Since the
portal doesn't know whether the last operation on a portlet was an
action or not, it would have no option but to repeat it over and over to
obtain the content of the portlet again (at least until the Credit Card
reached its limit).

Fortunately portals don't work that way. In order to prevent situations
like the one described above, the portlet specification defines two
phases for every request of a portlet, to allow the portal to
differentiate *when an action is being performed* (and should not be
repeated) and *when the content is being produced* (rendered):

-   **Action phase**: The action phase can only be invoked for one
    portlet at a time and is usually the result of an user interaction
    with the portlet. In this phase the portlet can change its status,
    for instance changing the user preferences of the portlet. It is
    also recommended that any inserts and modifications in the database
    or operations that should not be repeated are performed in this
    phase.

-   **Render phase**: The render phase is always invoked for all
    portlets in the page after the action phase (which may or not
    exist). This includes the portlet that also had executed its action
    phase. It's important to note that the order in which the render
    phase of the portlets in a page gets executedis not guaranteed by
    the portlet specification. Liferay has an extension to the
    specification through the element *render-weight* in
    liferay-portlet.xml. Portlets with a higher render weight will be
    rendered before those with a lower value.

In our example, so far we have used a portlet class called MVCPortlet.
That is all that the portlet if it only has a render phase. In order to
be able to add custom code that will be executed in the action phase
(and thus will not be executed when the portlet is shown again) you need
to create a subclass of MVCPortlet or directly a subclass of
GenericPortlet if you don't want to use the lightweight Liferay's
framework.

Our example above could be enhanced by creating the following class:

package com.liferay.samples;

\
\

import java.io.IOException;

import javax.portlet.ActionRequest;

import javax.portlet.ActionResponse;

import javax.portlet.PortletException;

import javax.portlet.PortletPreferences;

import com.liferay.util.bridges.mvc.MVCPortlet;

\
\

public class MyGreetingPortlet extends MVCPortlet {

\
\

@Override

public void processAction(

ActionRequest actionRequest, ActionResponse actionResponse)

throws IOException, PortletException {

\
\

PortletPreferences prefs = actionRequest.getPreferences();

\
\

String greeting = actionRequest.getParameter("greeting");

\
\

if (greeting != null) {

prefs.setValue("greeting", greeting);

prefs.store();

}\
\
 super.processAction(actionRequest, actionResponse);

}

}

\
\

The file portlet.xml also needs to be changed so that it points to our
new class:

<portlet\>

<portlet-name\>my-greeting</portlet-name\>

<display-name\>My Greeting</display-name\>

<portlet-class\>**com.liferay.samples.MyGreetingPortlet**</portlet-class\>

<init-param\>

<name\>view-jsp</name\>

<value\>/view.jsp</value\>

</init-param\>

…

\
\

Finally, you will need to do a minor change in the *edit.jsp* file and
change the URL to which the form is sent to let the portal know that it
should execute the action phase. This is the perfect moment for you to
know that there are three types of URLs that can be generated by a
portlet:

-   *renderURL*: this is the type of URL that we have used so far. It
    invokes a portlet using only its render phase.

-   *actionURL*: this type of URL tells the portlet that it should
    execute its action phase before rendering all the portlets in the
    page.

-   *resourceURL*: this type of URL can be used to retrieve images, XML,
    JSON or any other type of resource. It is often used to generate
    images or other media types dynamically. It is very useful also to
    make AJAX requests to the server. The key difference of this URL
    type in comparison to the other two is that the portlet has full
    control of the data that will be sent in response.

So we must change the *edit.jsp* to use an actionURL by using the JSP
tag of the same name. We also remove the previous code that was saving
the preference:

<%@ taglib uri="http://java.sun.com/portlet\_2\_0" prefix="portlet" %\>

<%@ taglib uri="http://liferay.com/tld/aui" prefix="aui" %\>

<%@ page import="com.liferay.portal.kernel.util.ParamUtil" %\>

<%@ page import="com.liferay.portal.kernel.util.Validator" %\>

<%@ page import="javax.portlet.PortletPreferences" %\>

<portlet:defineObjects /\>

\
\

<%

PortletPreferences prefs = renderRequest.getPreferences();

\
\

String greeting = (String)prefs.getValue(

"greeting", "Hello! Welcome to our portal.");

%\>

\
\

<portlet:**actionURL** var="editGreetingURL"\>

<portlet:param name="jspPage" value="/edit.jsp" /\>

</portlet:**actionURL**\>

\
\

<aui:form action="<%= editGreetingURL %\>" method="post"\>

<aui:input label="greeting" name="greeting" type="text" value="<%=
greeting %\>" /\>

<aui:button type="submit" /\>

</aui:form\>

\
\

<portlet:renderURL var="viewGreetingURL"\>

<portlet:param name="jspPage" value="/view.jsp" /\>

</portlet:renderURL\>

\
\

<p\><a href="<%= viewGreetingURL %\>"\>&larr; Back</a\></p\>

\
\

Try deploying again the portlet after making these changes, everything
should work exactly like before.

Well, almost. If you have paid close attention you may have missed
something, now the portlet is no longer showing a message to the user to
let him know that the preference has been saved right after clicking the
save button. In order to implement that we must have a way to pass
information from the action phase to the render phase, so that the JSP
can know that the preference has just been saved and then show a message
to the user.

## Passing Information from the Action Phase to the Render Phase

There are two ways to pass information from the action phase to the
render phase. The first one is through render parameters. Within the
implementation in the *processAction*method you can invoke the
*setRenderParameter* to add a new parameter to the request that the
render phase will be able to read:

actionResponse.setRenderParameter("parameter-name", "value");

From the render phase (in our case, the JSP), this value can be read
using the regular parameter reading method:

renderRequest.getParameter("parameter-name");

It is important to be aware that when invoking an action URL, the
parameters specified in the URL will only be readable from the action
phase (that is the *processAction* method). In order to pass parameter
values to the render phase you must read them from the actionRequest and
then invoke the *setRenderParameter* method for each parameter needed.

\

![image](../../images/03-portlet-development_html_5c790363.png)**Tip:** Liferay
offers a convenient extension to the portlet specification through the
MVCPortlet class to copy all action parameters directly as render
parameters. You can achieve this just by setting the following
init-param in your portlet.xml:\
\
<init-param\>

<name\>copy-request-parameters</name\>

<value\>true</value\>

</init-param\>

I mentioned there was a second method and in fact it is a better method
for what we are trying to do in our example. One final thing you should
know about render parameters is that the portal remembers them for all
later executions of the portlet until the portlet is invoked again with
different parameters. That is, if a user clicks a link in our portlet
and a render parameter is set, and then the user continues browsing
through other portlets in the page, each time the page is reloaded the
portal will render our portlet using the render parameters that we set.
If we used render parameters in our example then the success message
will be shown not only right after saving, but also every time the
portlet is rendered until the portlet is invoked again without that
render parameter.

The second method of passing information from the action phase to the
render phase is not unique to portlets so it might be familiar to you:
using the session. By using the session, your code can set an attribute
in the actionRequest that is then read from the JSP. In our case the JSP
would also immediately remove the attribute from the session so that the
message is only shown once. Liferay provides a helper class and taglib
to do this operation easily. In the processAction you need to use the
SessionMessages class:

package com.liferay.samples;

\
\

import java.io.IOException;

import javax.portlet.ActionRequest;

import javax.portlet.ActionResponse;

import javax.portlet.PortletException;

import javax.portlet.PortletPreferences;

**import com.liferay.portal.kernel.servlet.SessionMessages;**

import com.liferay.util.bridges.mvc.MVCPortlet;

\
\

public class MyGreetingPortlet extends MVCPortlet {

\
\

@Override

public void processAction(

ActionRequest actionRequest, ActionResponse actionResponse)

throws IOException, PortletException {

\
\

PortletPreferences prefs = actionRequest.getPreferences();

\
\

String greeting = actionRequest.getParameter("greeting");

\
\

if (greeting != null) {

prefs.setValue("greeting", greeting);

prefs.store();

}

\
\

**SessionMessages.add(actionRequest, "success");**

\
\

super.processAction(actionRequest, actionResponse);

}

}

Also, in the JSP you would need to add the liferay-ui:success JSP tag as
shown below (note that you also need to add the taglib declaration at
the top):

<%@ taglib uri="http://java.sun.com/portlet\_2\_0" prefix="portlet" %\>

<%@ taglib uri="http://liferay.com/tld/aui" prefix="aui" %\>

**<%@ taglib uri="http://liferay.com/tld/ui" prefix="liferay-ui" %\>**

<%@ page import="com.liferay.portal.kernel.util.ParamUtil" %\>

<%@ page import="com.liferay.portal.kernel.util.Validator" %\>

<%@ page import="javax.portlet.PortletPreferences" %\>

<portlet:defineObjects /\>

\
\

<%

PortletPreferences prefs = renderRequest.getPreferences();

\
\

String greeting = (String)prefs.getValue(

"greeting", "Hello! Welcome to our portal.");

%\>

\
\

**<liferay-ui:success key="success" message="Greeting saved
successfully!" /\>**

\
\

<portlet:actionURL var="editGreetingURL"\>

<portlet:param name="jspPage" value="/edit.jsp" /\>

</portlet:actionURL\>

\
\

<aui:form action="<%= editGreetingURL %\>" method="post"\>

<aui:input label="greeting" name="greeting" type="text" value="<%=
greeting %\>" /\>

<aui:button type="submit" /\>

</aui:form\>

\
\

<portlet:renderURL var="viewGreetingURL"\>

<portlet:param name="jspPage" value="/view.jsp" /\>

</portlet:renderURL\>

\
\

<p\><a href="<%= viewGreetingURL %\>"\>&larr; Back</a\></p\>

After this change, redeploy the portlet, go to the edit screen and save
it. You should see a nice message that looks like this:

![image](../../images/03-portlet-development_html_4540959e.png)*Illustration 1: The
sample “My Greetings” portlet showing a success message*

\
\

There is also an equivalent util class for notifying errors. This is
commonly used after catching an exception in the *processAction*. For
example.

try {

prefs.setValue("greeting", greeting);

prefs.store();

}

catch(Exception e) {

SessionErrors.add(actionRequest, "error");

}

And then the error, if it exists, is shown in the JSP using the
liferay-ui:error tag:

<liferay-ui:error key="error" message="Sorry, an error prevented saving
your greeting" /\>

When the error occurs you should see something like this in your
portlet:

![image](../../images/03-portlet-development_html_m606ce657.png)*Illustration 2: The
sample “My Greetings” portlet showing an error message*

\
\

The first message is automatically added by Liferay. The second one is
the one you entered in the JSP.

## Developing a Portlet with Multiple Actions

So far we have developed a portlet that has two different views, the
default view and an edit view. Adding more views is easy and all you
have to do to link to them is to use the *jspPage* parameter when
creating the URL. But we only have one action. How do we add another
action, for example for sending an email to the user?

You can have as many actions as you want in a portlet, each of them will
need to be implemented as a method that receives two parameters, an
ActionRequest and an ActionResponse. The name of the method can be
whatever you want since you will be referring to it when creating the
URL.

Let's rewrite the example from the previous section to use custom names
for the methods of the action to set the greeting and add a second
action.

public class MyGreetingPortlet extends MVCPortlet {

\
\

public void **setGreeting**(

ActionRequest actionRequest, ActionResponse actionResponse)

throws IOException, PortletException {

\
\

PortletPreferences prefs = actionRequest.getPreferences();

\
\

String greeting = actionRequest.getParameter("greeting");

\
\

if (greeting != null) {

try {

prefs.setValue("greeting", greeting);

prefs.store();

SessionMessages.add(actionRequest, "success");

}

catch(Exception e) {

SessionErrors.add(actionRequest, "error");

}

}

}

\
\

public void **sendEmail**(

ActionRequest actionRequest, ActionResponse actionResponse)

throws IOException, PortletException {

\
\

// Add code here to send an email

}

}

Note how we no longer need to invoke the processAction method of the
super class, because we are not overriding it.

This change of name also requires a simple change in the URL, to specify
the name of the method that should be invoked to execute the action. In
the edit.jsp edit the actionURL so that it looks like this:

<portlet:actionURL var="editGreetingURL" name="setGreeting"\>

<portlet:param name="jspPage" value="/edit.jsp" /\>

</portlet:actionURL\>

\
\

That's it, now you know all the basics of portlets and are ready to use
your Java knowledge to build portlets that get integrated in Liferay.
The next section explains an extension provided by Liferay to the
portlet specification to provide pretty URLs to your portlets that you
can use if desired.

## Optional: Adding Friendly URL Mapping to the Portlet

You will notice that when you click the *Edit greeting* link, you are
taken to a page with a URL similar to this:

http://localhost:8080/web/guest/home?p\_p\_id=mygreeting\_WAR\_mygreetingportlet&p\_p\_lifecycle=0&p\_p\_state=normal&p\_p\_mode=view&p\_p\_col\_id=column-1&\_mygreeting\_WAR\_mygreetingportlet\_jspPage=%2Fedit.jsp

In Liferay 6 there is a new feature that requires minimal work to change
this into:

http://localhost:8080/web/guest/home/-/my-greeting/edit

This feature, known as friendly URL mapping, takes unnecessary
parameters out of the URL and allows you to place the important
parameters in the URL path rather than the query string. To add this
functionality, first edit *liferay-portlet.xml* and add the following
lines directly after </icon\> and before <instanceable\>. Be sure to
remove the line breaks and the backslashes!

<friendly-url-mapper-class\>com.liferay.portal.kernel.portlet.Default\\

FriendlyURLMapper</friendly-url-mapper-class\>

<friendly-url-mapping\>my-greeting</friendly-url-mapping\>

<friendly-url-routes\>com/sample/mygreeting/portlet/my-greeting-friendly-url\\

-routes.xml</friendly-url-routes\>

Next, create the file (note the line break):

*my-greeting-portlet/docroot/WEB-INF/src/com/sample/mygreeting/portlet/my\\*

*-greeting-friendly-url-routes.xml*

Create new directories as necessary. Place the following content into
the new file:

<?xml version="1.0"?\>

<!DOCTYPE routes PUBLIC "-//Liferay//DTD Friendly URL Routes 6.0.0//EN"
"http://www.liferay.com/dtd/liferay-friendly-url-routes\_6\_0\_0.dtd"\>

<routes\>

<route\>

<pattern\>/{jspPageName}</pattern\>

<generated-parameter
name="jspPage"\>/{jspPageName}.jsp</generated-parameter\>

</route\>

</routes\>

Redeploy your portlet, refresh the page, and try clicking either of the
links again. Notice how much shorter and more user-friendly the URL is,
without even having to modify the JSPs. For more information on friendly
URL mapping, you can check full discussion of this topic in *Liferay in
Action*.
