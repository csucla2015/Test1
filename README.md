<a name="_Toc395888516"></a> <a name="_Toc395809325"></a> <a name="_Toc389865467"></a> <a name="_Toc389828008">Overview</a>
===========================================================================================================================

<a name="_Toc395888517"></a> <a name="_Toc395809326">Scenario</a>
-----------------------------------------------------------------

To highlight how customers can efficiently leverage Azure DocumentDB to
store and query JSON documents, this document provides an end-to-end
walkthrough of building a voting web application using Azure Document
DB.

This walkthrough shows you how to use DocumentDB service provided by
Azure to store and access data from an Python web application hosted on
Azure and presumes that you have some prior experience using Python and
Azure Websites.

You will learn:

1\. Creating and provisioning a DocumentDB Account

2\. Creating a Python MVC Application

3\. Connecting to and using Azure DocumentDB from your web application

4\. Deploying the Web Application to Azure Websites

By following this walkthrough, you will build a simple voting
application that allows you to vote for a poll.

Prerequisites
-------------

Before following the instructions in this article, you should ensure
that you have the following installed:

![\*][] Visual Studio 2013 (or [Visual Studio Express][] which is the
free version)

![\*][] Python for Visual Studio Beta tools from [here][]

![\*][] Azure SDK for Visual Studio 2013, version 2.4 or higher
available from [here][1]

![\*][] Azure Cross-platform Command Line Tools, available through
[Microsoft Web Platform Installer][]

<a name="_Toc395888519"></a> <a name="_Toc395809328">Create a DocumentDB database account</a>
=============================================================================================

To provision a DocumentDB database account in Azure, open the Azure
Management Portal and either Click the Azure Gallery tile on the
homepage or click “+” in the lower left hand corner of the screen.

This will open the Azure Gallery, where you can select from the many
available Azure services. In the Gallery, select “Data, storage and
backup” from the list of categories.

From here, select the option for Azure DocumentDB

Then select “Create” from the bottom of the screen

This will open up the “New DocumentDB” blade where you can specify the
name, region, scale, resource group and other settings for your new
account.

Once you’re done supplying the values for your account, Click “Create”
and the provisioning process will begin creating your database account.
Once the provisioning process is complete you should see a notification
appear in the notifications area of the portal and the tile on your
start screen (if you selected to create one) will change to show the
completed action.

Once provisioning is complete, clicking the DocumentDB tile from the
start screen will bring up the main blade for this newly created
DocumentDB account.

Using the “Keys” button, access your endpoint URL and the Primary Key,
copy these to your clipboard and keep them handy as we will use these
values in the web application we will create next.

  [\*]: PicExportError
  [Visual Studio Express]: http://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx
  [here]: https://pytools.codeplex.com/releases/view/123624
  [1]: http://go.microsoft.com/fwlink/?linkid=254281&clcid=0x409
  [Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
