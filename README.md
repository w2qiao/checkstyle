# Two ways of check redundant empty lines: Regex match or AST analysis.

## Regex match:

Add the following rule to xml file.

    <module name="RegexpMultiline">
        <property name="format" value="\r?\n[\t ]*\r?\n[\t ]*\r?\n"/>
        <property name="fileExtensions" value="java,xml,properties"/>
        <property name="message" value="Unnecessary consecutive lines"/>
    </module>
    
This will log a warning before the beginning of every two consecutive lines. But be careful to use regex match because the asymptotic complexity(or Big O) could be expotional, see [here](https://www.regular-expressions.info/catastrophic.html). Besides, there is a stackoverflow risk at `java.util.regex.Pattern`, see [here](https://github.com/checkstyle/checkstyle/blob/90f20e09869c71eb22190ad9c1d46d5deec324a5/src/main/java/com/puppycrawl/tools/checkstyle/checks/regexp/MultilineDetector.java#L114) 

## AST analysis

The three already existing rules for empty line checkers : 

- allowNoEmptyLineBetweenFields = true： Allow no empty line between fields.
- allowMultipleEmptyLines = true: Allow multiple empty lines between class members.
- allowMultipleEmptyLinesInsideClassMembers = true: Allow multiple empty lines inside class members.

For EVERY redundant empty lines, these checkers will report a warning using `getEmptyLinesToLog` method in `EmptyLineSeparatorCheck.java`. 

The existing checker doesn't check empty lines after class definition and the last class member. A workaround is to 
add `allowMultipleEmptyLinesInsideFile` checker to check all multiple empty lines inside a class file.

Note: `allowMultipleEmptyLinesInsideFile` works the same as regex match way, but it will log warnings at EVERY redundant empty lines.

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

# Check potentail hard code or magic number

A potential hard code is a integer or float number(except 0 and 1) used not in assignment expression. 
For example, 

`fooMethod(123, 456)`, `if(bar == 23762816)`, tokens like `123` `456` `23762816` are considered hard code,
These magic numbers sometimes could be confusing when reading codes.

add `allowNumNotInAssign` in `IllegalTokenTextCheck.java` to check these magic number as potential hard code.

How to use?
1. Add the following rule to xml file
    <module name="TreeWalker">
        <module name="IllegalTokenText">
            <property name="tokens" value="NUM_INT, NUM_FLOAT, NUM_DOUBLE, NUM_LONG"/>
            <property name="allowNumNotInAssign" value="false"/>
        </module>
    </module>
2. Complile checkstyle, use `mvn clean package -Passembly`,
3. Run java -jar checkstyle-8.8-SNAPSHOT-all.jar -c check_rule.xml Main.java 
Or download files from [here](https://drive.google.com/open?id=1KD2ajzfeI2uHsXQcFt1ly9QuUpgxiFG_)
