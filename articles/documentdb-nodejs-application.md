<a name="_Toc395783175">Building a Node.js Web Application using the Azure DocumentDB Service</a>
=================================================================================================

<a name="_Toc395783175">

This tutorial shows you how to use DocumentDB service provided by Azure
to store and access data from a [node][] application hosted on Azure.
This tutorial assumes that you have some prior experience using Node.js.

You will learn:

· How to use the Node.js Tools for Visual Studio

· How to work with Azure DocumentDB service using the node-documentdb
module

· How to deploy the web application to Azure Websites

By following this tutorial, you will build a simple web-based
task-management application that allows creating, retrieving and
completing of tasks. The tasks will be stored as JSON documents in Azure
DocumentDB.

<a name="_Toc395783176">Prerequisites</a>
-----------------------------------------

Before following the instructions in this article, you should ensure
that you have the following installed:

[Node][node].js version v0.10.29 or higher

[Git][]

Visual Studio 2013 with update 3 (or [Visual Studio Express][] which is
the free version)

[Visual Studio Tools for Node.js][]

Note : While we are using Visual Studio to build, debug, and deploy our Node.js
project in this tutorial you can use whichever IDE or editor you prefer
and run Node.js directly on whichever platform you choose in the way you
would normally run Node.js projects and then use the Azure CLI tools to
deploy your application to Azure Websites



<h2><a name="_Toc395783178">Create a new Node.js application</a></h2>

<p>In Visual Studio, select File – New Project and Select to create a “Basic Microsoft Azure Express Application”</p>

This will create a basic Express application for you. If you get
prompted to “install dependencies” select “Yes”. This will install all
the npm packages that are required for a new Express application.

Once this is complete the Solution Explorer should resemble the
following;

This shows you that you have Express, Jade and Stylus installed.

If you hit F5 in Visual Studio it should build the project, start
Node.js, and display a browser with the Express equivalent of “Hello
World”

<a name="_Toc395783179">Install additional modules</a>
------------------------------------------------------

The **package.json** file is one of the files created in the root of the
project. This file contains a list of additional modules that are
required for an Express application. Later, when you deploy this
application to an Azure Web Site, this file will be used to determine
which modules need to be installed on Azure to support your application.

We need to install two more packages for this tutorial.

Right click on “npm” in the Solution Explorer and select “Install npm
Packages”

In the “Install New npm Packages” dialog, type **nconf** to search for
the module. This module will be used by the application to read the
database configuration values from a configuration file.

Finally, install the Azure DocumentDB module in the same way by search
for **documentdb**. This is the module where all the DocumentDB magic
happens.

Once you have installed these two additional modules, and their
dependencies, they should show up in Solution Explorer under the **npm**
modules.

A quick check of the **package.json** file of the application should
show the additional modules, This file will tell Azure later which
packages it need to be download and installed when running your
application.

Edit the package.json file, if required, to resemble the example below.

This tells Node (and Azure later) that your application depends on these
additional modules.

<h1><a name="_Toc395783180">Using the DocumentDB service in a node application</a></h1>

<p>That takes care of all the initial setup and configuration, now let’s get down to why we’re here, and that’s to write some code using Azure DocumentDB. <br />
<br />
To start, edit <b>app.js</b> located in the root of the Express application we just created. <br />
Locate the following lines in the file;</p>

<p>And replace with the following two lines;</p>

<p>This tells the application what to do with the default GET and POST methods on a Form we will create next.</p>

<p>Now locate the <b>index.js</b> file found under the <b>routes</b> folder. Open this in your editor and delete all code found in this file.</p>

<p>Add the following at the top of the file;</p>

<p>This defines the modules that we are going to make use of; which are <b>documentdb</b> and <b>nconf</b>.</p>

<p><b>nconf </b>is a module that allows you to load configuration values, like database connection strings, from external files rather than placing these values in-line in your code. nconf will look for a <b>config.json</b> by default for its configuration.</p>

<p>Let’s go ahead and create an empty text file called <b>config.json</b> in the root of the project (same location as app.js) </p>

<p>Open this new <b>config.json</b> file and enter in the following values as appropriate for your DocumentDB endpoint. Be sure to set HOST and MASTER_KEY values correctly.</p>

<p>Now, switching back to <b>index.js</b>, add the following lines after the last lines to actually go read the configuration file and store the values in page level variables.</p>

<p>Now that this is done, continue adding the following code to <b>index.js</b></p>

<p>These are the two functions that we told the application to use earlier in the <b>app.js</b> when we defined the routes. When a GET hits the index view, the <b>exports.index</b> function will be run and similarly when a POST is received by the index view the <b>exports.createOfUpdateItem</b> function will be run.</p>

<p>These two functions do all the work of the application that we’re building however they call out to other functions, purely to make the code more readable and easier to follow. Continue adding the following code to the <b>index.js</b> file. This contains all the methods used by the two functions above and contain all the calls to DocumentDB. We will walkthrough each function in high-level detail later.</p>

<p><b>updateItem</b> – updates a document in the database based on the Item ID passed in from the form. It uses this ID to execute a <b><i>readDocument</i></b> method against DocumentDB to read the specific document we have stored. It then changes the “completed” attribute of the document to true, indicating it is now complete, and then proceeds to replace the document in the database. </p>

<p><b>getItem – </b>uses <b><i>queryDocuments </i></b>to get a single item from the database using the id property of the item.</p>

<p><b>createItem</b> – uses the <b><i>createDocument</i></b> method to create a new document in the database based on the Item Name &amp; Item Category entered on the form. It also sets the completed flag to false, to indicate this item is not yet completed.</p>

<p><b>listItems</b> – uses <b><i>queryDocuments</i></b> to look for all documents in the collection that are not yet complete, or where completed=false. It uses the DocumentDB query grammar which is based on ANSI-SQL to demonstrate this familiar, yet powerful querying capability. </p>

<p><b>readOrCreateDatabase</b> – uses <b><i>queryDatabases</i></b> to check if a database with this name already exists. If we can’t find one, then we go ahead and use <b><i>createDatabase</i></b> to create a new database with the supplied identifier (from our configuration file) on the endpoint specified (also from the configuration file)</p>

<p><b>readOrCreateCollection</b> – as with readOrCreateDatabase this method first tried to find a collection with the supplied identifier, if one exists it is returned and if one does not exist it is created for you. </p>

<p>That is all the code that is needed to interact with DocumentDB for this sample application.</p>

<p>Now let’s turn our attention to building the user interface so a user can actually interact with our application. The Express application we created uses <b>Jade</b> as the view engine. For more information on Jade please refer to <a href="http://jade-lang.com/">http://jade-lang.com/</a></p>

<p>Open the <b>layout.jade </b>filefound in the <b>views</b> folder and replace the contents with the following;</p>

<p>This effectively tells the <b>Jade</b> engine to render some HTML for our application and creates a “<b>block</b>” called “<b>content</b>” where we can supply the layout for our content pages.</p>

<p>Now open the <b>index.jade </b>file, the view associated with index.js and replace the content with the following; </p>

This extends layout, and provides content for the “block content”
placeholder we saw in the layout.jade file earlier.

In this layout we created two HTML forms.

\
 The first form contains a table for our data and a button that allows
us to update items.\
 The second form contains some input fields and a button that allows us
to create a new item.\
 When either of these buttons are clicked the **createOrUpdateItem**
(i.e. a form POST is done) the function we created in the **index.js**
file will be called. If this page is just loaded directly (i.e. a GET is
performed) then the **index** function is run.

This should be all that we need for our application to work.

</h1>
<a name="_Toc395783181">Run your application locally</a>
========================================================

To test the application on your local machine, hit F5 in Visual Studio
and it should build the app, start Node.js and launch a browser with a
page that looks like the image below:

1\. Use the provided fields for Item, Item Name and Category to enter
information, and then click **Add Item**.

2\. The page should update to display the newly created item in the ToDo
List.

1\. To complete a task, simply check the checkbox in the Complete column,
and then click **Update tasks**

</h1>
<a name="_Toc395783182">Deploy your application to Azure Websites</a>
=====================================================================

With the Node.js Tools for Visual Studio installed deploying to Azure
Websites is easily accomplished in a few short steps.

Right click on your project and select “Publish”

Then follow the publish wizard to provide the required configuration for
your Azure Website. The wizard will let you either choose an existing
website to update, or to create a new website.

Once you have supplied the necessary configuration just hit “Publish”

Then follow the publish wizard to provide the required configuration for
your Azure Website. The wizard will let you either choose an existing
website to update, or to create a new website.

Once you have supplied the necessary configuration just hit “Publish”

And Visual Studio will connect to your Azure subscription and publish
this Node.js application.

In a few seconds, Visual Studio will finish publishing your web
application and launch a browser where you can see your handy work
running in Azure!

<a name="_Toc395783183"></a> <a name="_Toc395637775">Conclusion</a>
===================================================================

Congratulations! You have just built your first Node.js Express Web
Application using Azure DocumentDB and published it to Azure Websites.

<a name="TutorialDownloadLink">The source code for the complete
reference application </a> can be downloaded [here][].

  [here]: http://go.microsoft.com/fwlink/?LinkID=509839&clcid=0x409

</h1>
  [node]: http://nodejs.org/
  [Git]: http://git-scm.com/
  [Visual Studio Express]: http://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx
  [Visual Studio Tools for Node.js]: https://nodejstools.codeplex.com/