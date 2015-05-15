---
layout: post
title: XML Validation with XSDs
date: 2009-06-10 19:31:42.000000000 -05:00
categories:
- Development
tags:
- ".NET"
- Excel
- Validate
- XML
- XSD
status: publish
type: post
published: true
meta:
  _edit_last: '1'
author:
  login: gibble
  email: chad.hurd@gcsquared.com
  display_name: caffgeek
  first_name: Chad
  last_name: Hurd
---
I've been doing a lot of work with excel uploads lately to allow clients to easily upload data to their systems.  Not the best approach mind you, but they know how to use excel...so what can you do?

What I have worked out is a multi step process to import the excel file to the database and validate the contents to a reasonable degree.  I have these all written into some custom classes and use IOC and DI to handle workflow so that writing subsequent uploads is a trivial task, and the majority of code is simply the validation and transformation files (XSDs, XSLs, and Stored Procs)

First, you have to retrieve the contents of the excel sheet(s)

{% highlight csharp %}protected XElement GetWorksheetXML(int sheetNumber, string xsltPath)
{
  // Create working file
  string workingFilePath = Path.GetTempFileName();
  File.WriteAllBytes(workingFilePath, this.filebytes);

  // Read the worksheet
  DataSet myDataSet = new DataSet();
  string strConn = "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" + workingFilePath + ";Extended Properties='Excel 8.0;HDR=No;IMEX=1;'";

  // Fill the Dataset
  OleDbDataAdapter myCommand = new OleDbDataAdapter("SELECT * FROM [" + GetExcelSheetNames(strConn)[sheetNumber] + "]", strConn);
  myCommand.Fill(myDataSet);

  // Clean up
  File.Delete(workingFilePath);

  // Return transformed XML
  return XElement.Parse(Utility.Transform(myDataSet.GetXml(), xsltPath, null));
}{% endhighlight %}

Secondly, convert it to a better XML format with an XSLT.  You should also merge any sheets here so you have one XML file from the XSL.  I'm not going to bother showing this, as it's hardly the point of the article, though, if you are unsure how to do this, feel free to ask

Third, and the whole point of this article, verify data types with XSD.

{% highlight csharp %}protected XElement ValidateXSD(XElement item, string xsdPath)
{
  XElement errors = new XElement("Errors");

  if (File.Exists(xsdPath))
  {
    // Reference: http://msdn.microsoft.com/en-us/library/bb358456.aspx
    XDocument xsd = XDocument.Load(xsdPath);
    XDocument xml = XDocument.Parse(item.ToString());
    XmlSchemaSet schemas = new XmlSchemaSet();
    schemas.Add("", XmlReader.Create(new StringReader(xsd.ToString())));
    xml.Document.Validate(schemas,
      // Validation Event/Error Handling
      (sender, e) =>
      {
        errors.Add(new XElement("Error", e.Message));
      }
    );
  }

  // If there were errors return them, otherwise return null
  return errors.Elements().Count() > 0 ? errors : null;
}{% endhighlight %}

Fourth, at this point, if there are no errors, you should pass the XML file as input to a stored procedure to further validate the data (if there are IDs and Codes that need to be verified with other systems), and assuming all has gone well, pass the XML file to a stored procedure which writes the data into your database...or whatever you need to do.  By this point your XML should be clean, and valid.