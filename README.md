# Welcome to Gecko385's homepage

This contains links to my GIT projects: 

## XML::Scraper

A Perl [module](https://github.com/gecko385/XML-Scraper) for parsing data out of XML using XPath expressions. The extract is specified in a YAML like format. This is used to (invisibly ) generate
the code to extract the data, which is returned to the application. Example:

```perl
!/usr/bin/perl
use XML::LibXML qw(:libxml);
use XML::Scraper;
use Data::Dumper::Concise;
use File::Slurp;
use YAML;
use JSON;

my $config_file = shift or die "need config file";
my $xml_file    = shift or die "need XML ";
my $output_file = shift or die "Need outout file";

my $config = YAML::LoadFile $config_file;
my $json = JSON->new->pretty(1);
my $dom = XML::LibXML->load_xml(location => $xml_file );
my $scraper = XML::Scraper->new;

write_file($output_file, $json->encode($scraper->parseDOM($dom,$config})));

print Data::Dumper::Concise::Dumper($scraper->getCode());
```

It does save a lot of time . Link to code:

[XML-Scraper](https://github.com/gecko385/XML-Scraper)

Here's a specification for pulling class definitions out of Doxygen C++ class XML files.

```YAML
classes :
    _class :                 "@class:findnodes:/doxygen/compounddef"
    class :
        id :                 "getAttribute:id"
        kind :               "getAttribute:kind"
        name :               "findvalue:./compoundname"

        _baseclasses :       "@base-class:findnodes:./basecompoundref[@prot=\"public\"]"
        base-class :
            name :           "to_literal:self"
            refid :          "getAttribute:refid"
        _includes :          "@file:findnodes:./includes"

        file :
            name :           "to_literal:self"
            refid :          "getAttribute:refid"

        _innerclass :        '@innerclass:findnodes:./innerclass[@prot="public"]'
        innerclass :
            name :            "to_literal:self"
            refid :           "getAttribute:refid"

        _pubtypes :          "@typedef:findnodes:./sectiondef[@kind=\"public-type\"]/memberdef[@kind=\"typedef\"]"
        typedef:
            type :          "findvalue:./type"
            definition :    "findvalue:./definition"
            argsstring :    "findvalue:./argsstring"
            name :          "findvalue:./name"

        _pub-enum :         "@enums:findnodes:./sectiondef[@kind=\"public-type\"]/memberdef[@kind=\"enum\"]"
        enums :
            type :           "findvalue:./type"
            name :           "findvalue:./name"
            _values :        "@values:findnodes:./enumvalue"
            values :
                name :        "findvalue:./name"
                initializer : "findvalue:./initializer"

        _pub-vars :           "@variables:findnodes:./sectiondef[@kind=\"public-attrib\"]/memberdef[@kind=\"variable\"]"
        variables :
            type :          "findvalue:./type"
            definition :    "findvalue:./definition"
            argsstring :    "findvalue:./argsstring"
            name :          "findvalue:./name"

        _pub-methods :      "@methods:findnodes:./sectiondef[@kind=\"public-func\"]/memberdef"
        methods :
            type :          "findvalue:./type"
            definition :    "findvalue:./definition"
            argsstring :    "findvalue:./argsstring"
            name :          "findvalue:./name"
            _params :       "@params:findnodes:./param"
            params :
                type :          "findvalue:./type"
                defname :       "findvalue:./defname"
                defval :        "findvalue:./defval"
                declname :      "findvalue:./declname"

        _pub-static-vars :   "@static-variables:findnodes:./sectiondef[@kind=\"public-static-attrib\"]/memberdef[@kind=\"variable\"]"
        static-variables :
            type :          "findvalue:./type"
            definition :    "findvalue:./definition"
            argsstring :    "findvalue:./argsstring"
            name :          "findvalue:./name"

        _pub-static-methods :      "@static-methods:findnodes:./sectiondef[@kind=\"public-static-func\"]/memberdef"
        static-methods :
            type :          "findvalue:./type"
            definition :    "findvalue:./definition"
            argsstring :    "findvalue:./argsstring"
            name :          "findvalue:./name"
            _params :       "@params:findnodes:./param"
            params :
                type :          "findvalue:./type"
                defname :       "findvalue:./defname"
                defval :        "findvalue:./defval"
                declname :      "findvalue:./declname"
```
