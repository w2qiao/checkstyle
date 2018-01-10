Add `allowMultipleEmptyLinesInsideFile` checker in `src/main/java/com/puppycrawl/tools/checkstyle/checks/whitespace/EmptyLineSeparatorCheck.java`
        
The already existing rules for empty line checkers : 

- allowNoEmptyLineBetweenFields = true： Allow no empty line between fields.
- allowMultipleEmptyLines = true: Allow multiple empty lines between class members.
- allowMultipleEmptyLinesInsideClassMembers = true: Allow multiple empty lines inside class members.

The existing checker doesn't check empty lines after class definition and the last class member. A workaround is to add 
add `allowMultipleEmptyLinesInsideFile` checker to check all multiple empty lines inside a class file.

For EVERY redundant empty line, these four checkers will report a warning using `getEmptyLinesToLog` method in `EmptyLineSeparatorCheck.java`. 

How to use?
1. Add the following rule to xml file.

        <module name="EmptyLineSeparator">
            <property name="allowMultipleEmptyLinesInsideFile" value="true"/>
        </module>

2. Complile checkstyle, use `mvn clean package -Passembly`,
3. Run java -jar checkstyle-8.8-SNAPSHOT-all.jar -c check_rule.xml Main.java 
Or download checkstyle-8.8-SNAPSHOT-all.jar check_rule.xml Main.java from [here](https://drive.google.com/open?id=1mTSimwVIztRT3Ma-c1ct_gypgplgx2hY)

