# Capabilities

The **Test Adapter for Catch2** has all the basic features one might expect of a test adapter for use with the test explorer in Visual Studio 2017. _I.e._, discovery of tests, running of tests with appropriate result messages, and starting a debug session for a test. Of course you can also set a working directory for running a test executable. In addition, some convenience features were added, such as a timeout for test cases and the ability to use a customized discovery mechanism.

The ability to use a [customized discovery mechanism](Settings.md#discovercommandline) basically enables the addition of a link to the location of the test case in the source code inside the detailed view for a test case.

Furthermore, the reported output is similar to that of the default output generated by Catch2. In case a test used `std::cout` and/or `std::cerr` for output, this output is accessible via the output link that is then enabled in the detailed view for the test case. This info is available regardless of whether the test passed or failed.

As of v1.1.0, the break on test failure feature of Catch2 (_i.e._, use the Catch2 command line option`--break`) is also supported by the **Test Adapter for Catch2**.

## Test Explorer specific

The Visual Studio Test Explorer enables grouping of test cases in various ways. To group test cases by Catch2 Tag, use the group by Traits option. In addition, by choosing appropriate test case names you can have your tests grouped by namespace and class. The extraction of a namespace and class from the test case name is also used in the hierarchical view of the Test Explorer. See [below](#notes-on-test-case-names) how this works.

## Notes on test case names

The Visual Studio Test Explorer tries to extract extra information from a test case name. Basically, it tries to split a test case name into three sections if possible: namespace, class, and description. The delimiters used for this are "." and "::". Here the last two delimiters in the test case name determine the split. See the table below for examples.

| Example | Namepsace | Class | Description |
|---------|-----------|-------|-------------|
| `std::vector. Test` | `std` | `vector` | `Test` |
| `Root.Level0.Level1.Name. Test 01` | `Root.Level0.Level1` | `Name` | `Test 01` |
|  `Root::Level0. Fraction=0.1` | `Root::Level0` |  `Fraction=0` |  `1` |

I suggest to experiment with this and (ab)use this functionality as appropriate for your case. Be aware though that trailing spaces are automatically removed from a test case name, and that names ending in a backslash ("\\") cannot be called by the **Test Adapter for Catch2**. Also, if you want to call a specific test case form the command line you need to escape any comma, double quote, open square bracket and backslash characters (_i.e._, use "\\,", "\\"", "\\[" and "\\\\"). This is basically what the **Test Adapter for Catch2** does internally when it calls a test.

In closing, test case names are semi case sensitive. Test cases which only differ in case are seperate test cases as far as Catch2 is concerned. However, they cannot be called individually via the command line. As such, when such a test is called, all tests with the same case insensitive name are run. At this time the **Test Adapter for Catch2** does not handle this corner case well and may therefore report the wrong result in that case.