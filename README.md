# Two ways of check empty line: Regex match or AST analysis.

## Regex match:

Add the following rule to xml file.

    <module name="RegexpMultiline">
        <property name="format" value="\r?\n[\t ]*\r?\n[\t ]*\r?\n"/>
        <property name="fileExtensions" value="java,xml,properties"/>
        <property name="message" value="Unnecessary consecutive lines"/>
    </module>
    
This will log a warning before the beginning of every two consecutive lines

## AST analysis

Add `allowMultipleEmptyLinesInsideFile` checker in `src/main/java/com/puppycrawl/tools/checkstyle/checks/whitespace/EmptyLineSeparatorCheck.java`
        
The three already existing rules for empty line checkers : 

- allowNoEmptyLineBetweenFields = true： Allow no empty line between fields.
- allowMultipleEmptyLines = true: Allow multiple empty lines between class members.
- allowMultipleEmptyLinesInsideClassMembers = true: Allow multiple empty lines inside class members.

For EVERY redundant empty lines, these checkers will report a warning using `getEmptyLinesToLog` method in `EmptyLineSeparatorCheck.java`. 

The existing checker doesn't check empty lines after class definition and the last class member. A workaround is to add 
add `allowMultipleEmptyLinesInsideFile` checker to check all multiple empty lines inside a class file.

Note: `allowMultipleEmptyLinesInsideFile` works the same as regex match way, but this will log warnings at EVERY redundant empty lines.

How to use?
1. Add the following rule to xml file.
        <module name="TreeWalker">
            <module name="EmptyLineSeparator">
                <property name="allowMultipleEmptyLinesInsideFile" value="true"/>
            </module>
        </module>

2. Complile checkstyle, use `mvn clean package -Passembly`,
3. Run java -jar checkstyle-8.8-SNAPSHOT-all.jar -c check_rule.xml Main.java 
Or download checkstyle-8.8-SNAPSHOT-all.jar check_rule.xml Main.java from [here](https://drive.google.com/open?id=1mTSimwVIztRT3Ma-c1ct_gypgplgx2hY)
