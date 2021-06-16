# Dynamic Date Example

The Dyanmic Date Example project demonstrates how to use a custom Method to support date calculation within an AML request. As an example, you could use the query below to find all items created 2 weeks ago. 

```xml
<created_on>TODAY() - 14</created_on>
```

## History

This project and the following release notes have been migrated from the old Aras Projects page.

Release | Notes
--------|--------
[v1.0.0](https://github.com/ArasLabs/dynamic-date-example/releases/tag/v1.0.0) | Initial Release

#### Supported Aras Versions

Project | Aras
--------|------
[v1.0.0](https://github.com/ArasLabs/dynamic-date-example/releases/tag/v1.0.0) | 12.0 SP15

> Though tested using Aras 12.0 SP15, this project only contains a single Server method, so it should be usable in any version of Innovator. 

## Installation

#### Important!
**Always back up your code tree and database before applying an import package or code tree patch!**

### Pre-requisites

1. Aras Innovator installed (version 12.0 SPx preferred)
2. Aras Package Import tool
3. Dynamic Date Example import package

### Install Steps

1. Backup your database and store the BAK file in a safe place.
2. Open up the Aras Package Import tool.
3. Enter your login credentials and click **Login**
  * _Note: You must login as root for the package import to succeed!_
4. Enter the package name in the TargetRelease field.
  * Optional: Enter a description in the Description field.
5. Enter the path to your local `..\dynamic-date-example\Import\imports.mf` file in the Manifest File field.
6. Select **aras.labs.DynamicDateExample** in the Available for Import field.
7. Select Type = **Merge** and Mode = **Thorough Mode**.
8. Click **Import** in the top left corner.
9. Close the Aras Package Import tool.

You are now ready to try out the Dynamic Date Example project.

## Usage

Any date properties included in an AML request using this project will be checked for the placeholder **TODAY()**. If that string is found, any additions or subtractions from that date will also be checked for and an appropriate date will be calculated. Valid examples of dynamic dates you can pass in can be seen below along with the expected output. Example calculations have been done relative to a date of January 1, 2000.

 Formula        | Computed Value 
----------------|------------------
TODAY()         | 2000-01-01
TODAY() + 1     | 2000-01-02
7 + TODAY()     | 2000-01-08
3 - TODAY()     | 1999-12-29
3 - TODAY() + 5 | 2000-01-03

The method code for this project was carefully written to support it's use as either a standalone action usable for any ItemType, or as an onBeforeGet server event on a specific ItemType.

### **Stand-alone Action**

Simply set the action of an AML request to **labs_CalculateDynamicDate**. After the dates are replaced, the action will perform a *get* request on the given ItemType with the calculated dates. The benefit to using a stand-alone action is that you can perform queries on any ItemType without needing to configure any additional options in the database.

```xml
<AML>
    <Item type="Part" action="labs_CalculateDynamicDate">
        <created_on>TODAY() - 7</created_on>
        <effective_date>TODAY() + 7</effective_date>
    </Item>
</AML>

<AML>
    <Item type="Document" action="labs_CalculateDynamicDate">
        <created_on>TODAY()</created_on>
        <effective_date>14 + TODAY()</effective_date>
    </Item>
</AML>
```

### **Server Event**

As an alternative, you can configure this same Method as an onBeforeGet server event on a specific ItemType or ItemTypes. 

1. Login to Innovator as an admin.
2. Navigate to **TOC > Administration > ItemTypes**
3. Search for and open the desired ItemType
4. Click the Edit button to edit the ItemType
5. Under the **Server Events** relationship tab, click the **Add Methods** button to add an existing Method
6. Set the **Event** to **onBeforeGet**
7. Save and close the ItemType
8. Repeat Steps 3-7 for any additional desired ItemTypes

After configuring this server event, you any *get* requests will be checked for a dyanmic date formula.

```xml
<AML>
    <Item type="Part" action="get">
        <created_on>TODAY()</created_on>
    </Item>
</AML>
```

## Enhancement Ideas

1. Add options for additional date functions
   1. e.g. **DATE(1, 1, 2000)** to get a specific date instead of just today
2. Add ability to process more than just day additions
   1. e.g. **TODAY() + 1M** would add one month instead of just one day
3. Extend functionality to work on *Add* and *Edit* requests

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request

For more information on contributing to this project, another Aras Labs project, or any Aras Community project, shoot us an email at araslabs@aras.com.

## Credits

Project written, documented, and published by Christopher Gillis at Aras Labs. @csgillis-aras

## License

Aras Labs projects are published to Github under the MIT license. See the [LICENSE file](./LICENSE.md) for license rights and limitations.